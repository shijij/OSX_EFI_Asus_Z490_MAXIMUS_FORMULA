# Hackintosh OpenCore EFI For Asus Z490 MAXIMUS XII FORMULA
* **CPU**: Intel 10700k
* **MB**: Asus ROG MAXIMUS XII FORMULA (Z490)
    * **LAN 1: Intel® I225-V 2.5Gb** *[PciRoot(0x0)/Pci(0x1C,0x4)/Pci(0x0,0x0)]*
    * **LAN 2: Marvell® AQtion AQC107 10Gb** *[PciRoot(0x0)/Pci(0x1C,0x6)/Pci(0x0,0x0)]*
    * **Audio**:  SupremeFX CODEC S1220 (**Realtek ALC1220**) *[PciRoot(0x0)/Pci(0x1F,0x3)]*
    * **Wifi**: Intel AX201 (No Driver) *[PciRoot(0x0)/Pci(0x14,0x3)]*
* **Mem**: Corsair Vengeance LPX 64GB DDR4 DRAM 3200MHz (CMK64GX4M2E3200C16)
* **GPU**: Sapphire Pulse Radeon RX 5700 XT 
* **SSD**: 
    * Win: M2 EVO 1T
    * Mac (10.15.5): M2 EVO Plus 1T

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

## Failed parts:
Intel Wifi and BT
* Purchasing a BCM94360CD will make them work. 
* LAN is enough for me.

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

Tried a few more times to reproduce the crash but no luck.

(Power Nap is disabled)
 
#### Unstable DP audio output
In a 10 seconds timeframe, there will be 0.5 seconds with no sound on average.

### More bug?
I will keep you posted.