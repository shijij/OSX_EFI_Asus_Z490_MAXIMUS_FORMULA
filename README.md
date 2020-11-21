# Hackintosh OpenCore EFI For Asus Z490 MAXIMUS XII FORMULA

## Alert:
**This hardware configuration is not perfect because there isn't any iGPU support for this motherboard.**

**Avoid ROG MAXIMUS XII EXTREME/FORMULA/APEX if you need Sidecar/iGPU etc..**

**iGPU for these 3 boards aren't powered at all.**

But anyway, I've left the relative configs there already. 
For more help on iGPU setup, please visit [Here](https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#deviceproperties)



* **CPU**: Intel 10700k
* **MB**: Asus ROG MAXIMUS XII FORMULA (Z490)
    * **LAN 1: Intel® I225-V 2.5Gb** *[PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)]*
    * **LAN 2: Marvell® AQtion AQC107 10Gb** *[PciRoot(0x0)/Pci(0x1C,0x6)/Pci(0x0,0x0)]*
    * **Audio**:  SupremeFX CODEC S1220 (**Realtek ALC1220**) *[PciRoot(0x0)/Pci(0x1F,0x3)]*
    * **Wifi**: Intel AX201 (No Driver & Not in use) *[PciRoot(0x0)/Pci(0x14,0x3)]*
* **Mem**: Corsair Vengeance LPX 64GB DDR4 DRAM 3200MHz (CMK64GX4M2E3200C16)
* **GPU**: Sapphire Pulse Radeon RX 5700 XT 
* **SSD**: 
    * Win: M2 EVO 1T
    * Mac: M2 EVO Plus 1T
* **OS** 
    * ~~Catalina 10.15.5~~ 
    * ~~Catalina 10.15.6~~
    * Catalina 10.15.7
* **Bootloader** 
    * OpenCore 0.6.3 
* **Additional Hardware**
    * Wifi & BT: BCM94360CD (Fenvi branded)

# Credit
[@jergoo](https://github.com/jergoo/)
[https://github.com/jergoo/Hackintosh-ROG-STRIX-Z490I/](https://github.com/jergoo/Hackintosh-ROG-STRIX-Z490I/)
This repo was used in my MacOS installation flash drive. I just changed a few things based on it.

# Tools used:
* OpenCore Configurator
* Hackintool


# The following changes are made:
(on jergoo's repo)
## With OpenCore Configurator
* [NVRAM -> 7C43...] boot-args:
    * [Update] agdpmod=pikera  
        * Reason: agdpmod was set to 'ignore', only one HDMI and one DP have signal.
    * [Append] darkwake=2
        * Reason: 'Sleep' will shutdown the system
* [DeviceProperties]
    * [Rename] change the PCIe address of the Intel I225-V
    * [Update] layout id for S1220(ALC1220) is 1 on this MB
* [Kernel]
    * [Disable] USBPorts.kext
    * [Enable] USBPorts-All.kext
    
## With Hackintool
* [Power] -> Fix Sleepimage
    *  Reason: 'Sleep' will shutdown the system

## Verified working parts/functions/apps.
* Dual Display (Video and Audio, on HDMI and DP)
* Sleep/Wake-up/Shutdown/Restart
* Two LANs under 1000Mpbs
* Audio output from ALC1220, with Boom3D
* GPU (5700xt)
* Most USB ports (Speed test not performed)

## Problems:
* iGPU and Sidecar are not working. due to the nature of this motherboard design.

## Known bugs
#### Conditionally No Sound from ALC1220 
* When you
    * Installed Windows in another disk
    * AND installed the additional Realtek Audio Driver
    * AND warm restarted into MacOS from Windows

Ref: [Discussion from tonymacx86](https://www.tonymacx86.com/threads/solved-no-audio-after-reboot-from-windows-applehda-w-alc-668.187624/)

Fix: Cold restart when switching into MacOS from Win.

#### Kernel panic cause by GPU
com.apple.kext.AMDRadeonX6000Framebuffer

This happened once when I power on my second LCD screen. Kernel crashed followed by a restart.

To reproduce it, you will need more than one monitor. randomly toggle their power buttons quickly will trigger this error.

So, do not trigger too many desktop resize(when a display is turned on/off/unplugged/replugged) in a short period of time. I would say 3 times within 5 seconds is dangerous. 

(Power Nap is disabled)


 
#### Unstable DP audio output
In a 10 seconds timeframe, there will be 0.5 seconds with no sound on average.

### More bug?
I will keep you posted.
