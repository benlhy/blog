---
title: "Getting Started with Atollic TrueStudio and STM32"
date: 2018-12-14T03:26:41+00:00
slug: "getting-started-with-atollic-truestudio-and-stm32"
draft: false
description: "Exploring the STM32, a family of Arm Cortex chips from ST Microelectronics!"
cover:
  image: "/images/2018/12/IMG_20181224_090358_HDR.jpg"
  relative: false
  hiddenInList: false
tags: ["STM32", "Getting Started", "Atollic", "ST Microelectronics"]
---

> Tldr; I really like the STM32 and you should definitely give it a whirl.

> UPDATE [12 October 2019]: ST just integrated Atollic into their CubeMX branding, and it is now known as[ STM32CubeIDE](https://www.st.com/en/development-tools/stm32cubeide.html). It is essentially the same as Atollic TrueStudio with proper STM32CubeMX integration. I wrote a [brief post about this](https://westsideelectronics.com/stm32cube-ide/).

Recently, ST Microelectronics bought Atollic and released [**TrueStudio**](https://atollic.com/truestudio/) as a free Integrated Development Environment (IDE) for their STM32 line. The STM32 line is a 32-bit microcontroller line from ST Microelectronics utilizing the ARM Core. This is part of same series of cores that you see in the ubiquitous SAMD21 development boards.

This is great as there is finally one official IDE that ST provides, which means dedicated support for STM32 chips and boards. No more twiddling and configuring to get Eclipse to work as a STM32 IDE! To me, this is a signal to jump into STM32 chips.

# Why STM32?

Think Arduino, and what you have is an ATMEGA328P. This is an 8-bit AVR microcontroller that is mostly limited to the microcontroller world. The next step up is the ARM core, it is a 32-bit microprocessor in everything from the tiniest microcontroller to phones and laptops. Theoretically, once you learn the architecture of ARM core, there is nothing stopping you from moving up and down the chain, which makes it very attractive to learn, since it can be applied to such a wide latitude of chips. The ARM core is a design that is licensed out to manufacturers, so there are quite a number of manufacturers producing ARM chips. One commonly known microcontroller is the SAMD21 series manufactured by Atmel/Microchip, popularized by Adafruit in their CircuitPython line of boards.

For ST Microelectronics, the STM32 line is their main 32-bit microcontroller line, and they were one of the first few to adopt the ARM microprocessor in their designs, meaning that the STM32 chips have been around for some time. Age is positively related to reliability for most microcontrollers because the bugs would have been worked out over time.

ST provides excellent and free software support in terms of the compilers, IDE plugins, and most recently, a fully fledged IDE for their microcontrollers. This is of interest to hobbyists and students because the tooling can add up to quite a bit. Take Microchip for instance, where you would have to pay a monthly fee of \$30 to get a better compiler with all the optimizations, or a one time fee of a \$1000.

# The STM32 ecosystem

TrueStudio is a Ellipse-based IDE that is modified to support programming and debugging STM32 chips. Programming in it feels fast, and the learning curve is not that high, since most of the configuration options such as the compiler are taken care of by Atollic. It usually goes as: `Create project in CubeMX > Switch to C perspective for programming > Click on debug for flashing and debugging`. I highly recommend installing TrueStudio and adding the STM32CubeMX plugin.

{{< figure src="/images/2018/12/image-1.png" alt="" caption="Atollic TrueStudio" >}}

ST seems to be a cost-efficient choice because it is the dominant chip that is used for many flight controllers. There are a few things that a flight controller must do well:

- Process data really quickly
- Fast peripherals connecting sensors and data collection.

Knowing that these features are supported makes evaluating ST chips very attractive to me.

{{< figure src="/images/2018/12/image-7.png" alt="" caption="Generic Flight Controller using an STM32F3 chip" >}}

There is an extremely low cost development board offered by sellers on AliExpress called the **STM32 Blue Pill** that packs an M3 core in the form of a STM32F103 chip and costs less than a cup of coffee. The bare chips themselves are \$1 in single quantities from China. Let's think about it for a second: that is an *ARM Cortex M3 chip* that is *well supported in documentation and software* for **prices that are practically throwaway**. Installing one of these boards in a project is no problem at all. For comparison, an ATTINY85 costs about \$1 from the same sources.

{{< figure src="/images/2018/12/image-2.png" alt="" caption="STM32 Blue Pill" >}}

The programmer, known as the **ST-Link V2**, is \$25 from ST or \$5 for its clones, or even free if you buy a **Nucleo**-style development board from ST. Most **Nucleo** board consist of two parts: the board hosting the microcontroller, and the board hosting the programmer, which breaks out the programming pins that you can then use to program other boards, like the aforementioned Blue Pill.

{{< figure src="/images/2018/12/image-5.png" alt="" caption="STM32 Nucleo" >}}

> After some exploration, I noted that the ST-Link V2 clones from China actually supply 3.3V and 5V instead of just measuring the target voltage. This can be convenient sometimes, but it might not be a good feature when programming new boards where the power circuitry is not tested.

One of the most common chips you will see is the STM32F103C8T6. This is available on the Blue Pill boards and used as the programmer chip on Nucleo boards.

### Comments

Some pointers for using the ST-Link Utility when flashing hardware. If it crashes without any error warnings on Windows, you might have to run it as an administrator.

# STM32CubeMX

Another very nice feature of the STM32 is that ST provides a software called **STM32CubeMX. **It provides a graphical interface that automatically generates code for initialization for pins and peripherals. This is very convenient means to set up a project quickly. This program also integrates with TrueStudio and it forms a very nice workflow where you generate code in the **STM32CubeMX **perspective and switch over to the C++ perspective when you are done, and then to the debug perspective once your code is written.

It also includes tags to indicate where code should be written so that when you add new features, for example, adding SPI to your project, it doesn't overwrite your existing code.

## Setting up a new project for a bare chip

This is the procedure that I use when programming a new chip.

- Under SYS, enable Serial Wire debug
- Under RCC, enable external clocks
- In Clock Configuration, configure clocks
- Under Project Manager, change Toolchain/IDE to TrueStudio

### Little gotchas when using STM32CubeMX

- When setting up a bare MCU/Blue Pill, you might come across an issue where an error message such as **USB communication error**, or **Target is not responding** crops up. Check that you have Debug (SWCLK/SWDIO) pins defined when you are setting up STM32CubeMX (check the ***Debug->Serial Wire *option SYS**). I didn't, and I couldn't figure out why my board could only be programmed once for quite some time, and I had to reprogram it each time by erasing the memory through the STLink Utility while holding and releasing the RESET button.
- I had an error were I accidentally set the project up as an EWARM project and tried to switch it to a TrueStudio project. This caused an error in the arm_math.h library where it read **"*Define according the used Cortex core...*"**. However, when **I created a new project, this error went away**. This error also occurred when I copied projects and tried to change the target MCU.
- Occasionally when STM32CubeMX detects changes have been made and asks if you would like to save the project, **clicking *yes *sometimes causes TrueStudio to crash.**

# Libraries

Having an existing library is pretty important to me because it abstracts away the technicalities of the hardware and allows you to reuse your code. It also allows you to get started with new hardware quickly.

ST provides the **Hardware Abstraction Layer (HAL) library** which makes development very easy across different families of the STM32.

{{< figure src="/images/2018/12/image-3.png" alt="" caption="STM32Cube" >}}

Some other reviews that people have done of the STM32 line are **[Jay Clarson](https://jaycarlson.net/pf/st-stm32f0/), **author of *The Amazing $1 Microcontroller. *I highly recommend reading this article because it highlights the various aspects of each chip from a wide array of vendors.

Now that we have the software set up, it is time to test the hardware. While, I have my **[Proving Fields](https://westsideelectronics.com/proving-fields/) **board, in this case I decided to prototype it on a breadboard.

## Programming

This records my experiences with prototyping on a breadboard. There is a system that I use to get used to a new chip:

- Toggle GPIO
- Get SPI working with the MCP4192
- Get I2C working with the MCP23008
- More complex SPI with ST7735
- PWM
- Interrupts with GPIOs
- Timer Interrupts

## Adventures with SPI

### MCP23008

Lessons learned:

- Make sure that you are working with the right chip. Because MCP23008 comes in two flavors (SPI/I2C), I tried to program it in SPI, but the SPI version is actually MCP23S08. So I wasted a lot of time trying to use the wrong protocol on a chip that doesn't support it.
- I wired the Latch pin to high, but in order for the output to actually switch when the slave select pin goes high (indicating that the transaction is complete), the Latch pin should be wired to low.

These problems could have been avoided with the Proving Fields board. There would also be less time wasted cutting wires, wiring up, and double checking connections.

### ST7735 LCD

Lessons learned:

- Tie Reset to **High** if you don't care about resetting the screen.
- STM32CubeMX initializes some strange defaults for SPI, so for future reference: **Set bits per transfer to 8 bits,** and **set a pull-down on the Clock Pin**.
- The Proving Fields board MOSFET for turning the screen on and off still doesn't work. Related to how I put it on the low side instead of the high side. The fix is to remove the MOSFET and directly connect the pin low to turn on the LED.

## Squaring off with I$^2$C

Lessons learned:

- When writing down the address, make sure to shift it by one to the left so as to accommodate the read/write bit that will automatically be set by the HAL library.

```
  uint16_t addr = 0b0100111<<1;
  uint16_t size = 2;
  uint32_t timeout = 5000;
  uint8_t pData[size];
  pData[0] = IODIR;
  pData[1] = 0x0;
  HAL_I2C_Master_Transmit(&hi2c1, addr, pData, size, timeout);
```

## Yakking with UART

I used the Direct Memory Access (DMA) to test UART and it worked quite well. DMA is a co-processor that reduces the load on the processor by shifting incoming characters on UART to a temporary buffer to be processed later. The processor will not then be continuously interrupted, especially at high UART rates.

One limitation is that we have to specify the number of bytes DMA should transfer before it stops. This means that we have to know the length of the data transfer beforehand.

## Pinging PWM

- To enable the complementary PWM output on Timer16, you have to call the following line in HAL:

```
HAL_TIMEx_PWMN_Start(&htim16, TIM_CHANNEL_1);
```

## Wrangling with USB

USB configuration was quite smooth with the STM32CubeMX. Surprisingly so , given how complex USB configuration options can be sometimes (looking at you Microchip). Note that I am working with the Blue Pill.

- Enable the USB peripheral as well as the USB middleware, selecting Virtual Port Com.
- Go to the generated code. Under `usbd_cdc_if.c`, edit the `CDC_Receive_FS` function so that the board does something when you send a character.

```
if(Buf[0]=='1') {
	HAL_GPIO_WritePin(LED_GPIO_Port,LED_Pin,GPIO_PIN_RESET);
}
else {
	HAL_GPIO_WritePin(LED_GPIO_Port,LED_Pin,GPIO_PIN_SET);
}
```

Now upload the code to the board, your computer should detect that a COM port is connected and when you send 1, the LED (active low) should turn on, and any other character would cause the LED to turn off.

Sending information is as simple as adding the `CDC_Transmit_FS()` to the `main()` loop.

# Impressions thus far

{{< figure src="/images/2018/12/stm32-1.jpg" alt="" caption="Breadboarding the STM32" >}}

I am pleasantly surprised at how easy it is to get started with the STM32CubeMX software. I was expecting bugs or to have to initialize some functions myself, but by and large, the initialization works well and without errors. I was able to get most of the basics working after about two days of work, or effectively five hours if you leave out the mistakes I made during wiring.

The main documentation is a little thinner than what I am used to from Microchip or TI, and it might be that they are broken up into different sets of datasheets, each relating to a specific topic such as the ADC operation or SPI. It is still slightly annoying to have to hunt all over ST's website for the datasheet specific to the task that I want to do, but I have a feeling that I will be spending less time doing this with the STM32 microcontrollers simply because of HAL and STM32CubeMX.

I would certainly recommend getting familiar with the STM32. Development is well supported and there is a huge potential latitude in the STM32 family.
