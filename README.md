# thingBot-SubGHz

The thingBot-SubGHz is based on TI's CC1310 SoC (System on Chip), featuring an ARM Cortex-M3 running at 32 MHz and with 20 kbytes of RAM and 128 kbytes of FLASH. It has the following key features:

  * Standard Cortex-M3 peripherals (NVIC, SCB, SysTick)
  * Sleep Timer (underpins rtimers)
  * SysTick (underpins the platform clock and Contiki's timers infrastructure)
  * Deep Sleep support with RAM retention for ultra-low energy consumption
  * Support for CC13xx prop mode: IEEE 802.15.4g-compliant sub-GHz (868 MHZ) operation
  * UART
  * 2xSPI, I2C
  * Real-Time Clock (RTC)
  * Watchdog (in watchdog mode)
  * uDMA Controller (RAM to/from USB and RAM to/from RF)
  * True Random Number Generator (TRNG)
  * Support for Eight Capacitive Sensing Buttons
  * Low Power Modes
  * General-Purpose Timers
  * 8-channel ADC
  * Cryptoprocessor (AES-ECB/CBC/CTR/CBC-MAC/GCM/CCM-128/192/256, SHA-256)
  * Ultra-Low-Power Sensor Controller
  * Supports Over-the-Air (OTA) Update
  * All Digital Peripheral Pins Can Be Routed to Any GPIO
  * Flash-based port of Coffee
  * PWM
  * Built-in core temperature and battery sensor
  
## Platform Features

  * [XBee Compatible](https://www.sparkfun.com/datasheets/Wireless/Zigbee/XBee-Dimensional.pdf)
  * Compatible with Contiki OS
  * c-JTAG (10 pin Cortex-M connector)
  * On-board LEDs
  * 3.3V Vin
  * UART, SPI, I2C
  * 3 ADCs
  * On-board RESET and USER button
  * On-board Sensors (Temperature, Humidity, Ambient Light)
  
## Getting Started with Contiki for TI CC13xx

This guide's aim is to help you start using Contiki for TI's CC13xx.

The CPU code can be found under `$(CONTIKI)/cpu/cc26xx-cc13xx`.
The port was developed and tested with CC1310s and any bug reports are welcome.
Bear in mind that the CC1310 does not have BLE capability.

This port is only meant to work with 7x7mm chips

This guide assumes that you have basic understanding of command line interface (CLI) 
and perform basic admin tasks on UNIX family OSs.

### To use the port you need:

* TI's CC13xxware sources. The correct version will be installed automatically
  as a submodule when you clone Contiki.
* Contiki can automatically upload firmware to the nodes over serial with the
  included [cc2538-bsl script](https://github.com/JelmerT/cc2538-bsl).
  Note that uploading over serial doesn't work for the Sensortag, you can use
  TI's SmartRF Flash Programmer in this case.
* A toolchain to build firmware: The port has been developed and tested with
  GNU Tools for ARM Embedded Processors (<https://launchpad.net/gcc-arm-embedded>).
  The current recommended version and the one being used by Contiki's regression
  tests on Travis is shown below.

        $ arm-none-eabi-gcc --version
        arm-none-eabi-gcc (GNU Tools for ARM Embedded Processors) 5.2.1 20151202 (release) [ARM/embedded-5-branch revision 231848]
        Copyright (C) 2015 Free Software Foundation, Inc.
        This is free software; see the source for copying conditions.  There is NO
        warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.

* SRecord (http://srecord.sourceforge.net/)

Follow this checklist for enabling the SRF06 EB to be able to flash Contiki OS:

1) Connect SRF06 EB to a linux computer and install FTDI drivers as mentioned in
[SmartRF06 EB](https://github.com/contiki-os/contiki/blob/master/platform/cc2538dk/README.md#for-the-smartrf06-eb-uart)
This step is same for all boards working with SRF06 EB.

2) Now go to contiki/cpu/cc26xx-cc13xx/lib and go to appropriate folder either cc13xxware. 
Go to startup_files and open ccfg.c and make changes to the file as specified in
[cc26xx and cc13xx](https://github.com/JelmerT/cc2538-bsl/blob/master/README.md#cc26xx-and-cc13xx)

3) Now you can flash Contiki Code using `sudo make TARGET=srf06-cc26xx BOARD=srf06/cc13xx {filename}.upload PORT=/dev/ttyUSB1`
to flash the firmware onto the cc13xx devices. 
You might need to hold on to __SELECT BUTTON__ and press __EM RESET__ before flashing to go into bootloader mode. 
After the first time, the device automatically goes into this mode whenever you are flashing the code. 
__USB__ port must be appropriately selected. 
Generally __ttyUSB0__ is for __XDS100v3__ and __ttyUSB1__ for the __TARGET__ device.

### Use the Port

The following examples are intended to work off-the-shelf:

* Examples under `examples/cc26xx`
* Border router: `examples/ipv6/rpl-border-router`
* Webserver: `examples/webserver-ipv6`
* CoAP example: `examples/er-rest-example`
* IPSO example: 'examples/ipso-objects`

### Build your First Examples

It is recommended to start with the `cc26xx-demo`, 
it is a simple example that demonstrates the thingBot-SubGHz features, such as the built-in sensors, LEDs, user button and radio.

The `Makefile.target` includes the `TARGET=` argument, predefining which is the target platform to compile for, it is automatically included at compilation.

To generate or override an existing one, you can run:

`make TARGET=srf06-cc26xx BOARD=srf06/cc13xx savetarget`

If you want to upload the compiled firmware to a node via the serial boot loader you need first to either manually enable the boot loader.

Then use `make TARGET=srf06-cc26xx BOARD=srf06/cc13xx cc26xx-demo.upload PORT=/dev/ttyUSB1`. 

The `PORT` argument could be used to specify in which port the device is connected, in case we have multiple devices connected at the same time.

## Maintainers

The thingBot-SubGHz is maintained by thingTronics Innovations.

Main contributor:
 * Lovelesh Patel @<lovelesh.patel@thingtronics.com>
 * Vishnu Sherikar @<vishnu.sherikar@thingtronics.com>