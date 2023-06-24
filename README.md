# Hackintosh on ASUS X99 Deluxe II

Supports Vetuna 13.4.1, Works fine.

Using OpenCore 0.9.3 with all kexts up to date.

![screenshots](./screenshots/%E6%88%AA%E5%B1%8F2023-06-25%2002.24.04.png)

## Setup:

- Asus X99 Deluxe II BIOS: Ver. 2101 Modified Version (Seeing below)
- Core i7 5820K 3.3 GHz, Haswell-E
- SAPPHIRE NITRO+ AMD Radeon RX 580 8GB
- 16GB DDR4 2133MHz
- Toshiba OEM NVMe SSD 500GB (From my XPS 15 9570)
- Samsung PM981 512G (Cannot work under macOS, blocked using SSDT)

## Feature:

- Sleeping
- Wi-Fi/Bluetooth, Airdrop/Handoff/Airplay/Unlock by Watch... All works well.
- Hardware acceleration (has tested Apple Music Lossless)
- USB Mapped*
- Intel I218-V & I211-A

## Not Tested

- Thunderbolt

## Known Issues

- Random restart when booting
- Front panel USB3_34 has been deleted due to 15 ports limits. (since I have no need to use it)

## Pre-Installation

### BIOS

Thanks to [psymbionic at tonymacx86](https://www.tonymacx86.com/threads/monterey-public-beta-on-asus-x99-deluxe-ii.314207/post-2270958), there's a latest 2101 version BIOS that has MSR unlocked.

If you gonna trying to use USB Flaskback your BIOS, notice that your USB drive should be MBR. A USB disk with GPT could not work.

If you do not want to flash BIOS, turned on these two quirks:

- `Kernel -> Quirks -> AppleCpuPmCfgLock`
- `Kernel -> Quirks -> AppleXcpmCfgLock`

## Installation

Install macOS Ventura normally following [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)

### Emulated NVRAM

Someone says that X99 Deluxe II's native NVRAM could working under macOS but needs BIOS patching. I cannot find the way to do it. And I could not make it working either. So you still need emulated NVRAM.

[Follow this guide to set up a emulated NVRAM.](https://dortania.github.io/OpenCore-Post-Install/misc/nvram.html)

### Sleeping

Disable macOS advenced features which not functional under Hackintosh.

```shell
sudo pmset autopoweroff 0
sudo pmset powernap 0
sudo pmset standby 0
sudo pmset proximitywake 0
sudo pmset tcpkeepalive 0
```

### Spoofing CPU Model

If your CPU is also Haswell-E like me, you don't need to change anything.

If your CPU is Broadwell-E, change `Kernel -> Emulate -> Cpuid1Data` to `D4060300 00000000 00000000 00000000`

You wouldn't turn on native power management if CPU model are missmatch.

### SSDTs

There are 2 SSDTs which you need to pay some attention.

#### SSDT-NVMe-extern-icon-patch

I found that my NVMe SSD showed as external when I first finished installation. So I use an SSDT to fix that. But you need to change the device path based on your setup.

Use Hackintool to find your SSD's device path, and use MaciASL to modify it.

#### SSDT-PM981-DISABLE-DELUXE-II

Samsung PM981 could not work under macOS, and system could not even boot most of the time on my machine. I disabled it by using an SSDT. If you don't need it, remove it simply.

But if you need it, please change device path based on your setup.

### iServices

[Fixing iServices](https://dortania.github.io/OpenCore-Post-Install/universal/iservices.html#using-gensmbios)

## Thanks to

[Apple](https://www.apple.com) for macOS

[acidanthera](https://github.com/acidanthera) for providing almost all kexts and drivers

[RehabMan](https://github.com/RehabMan) for his contribute to Hackintosh community, we all miss him.

[CaseySJ](https://www.tonymacx86.com/threads/monterey-public-beta-on-asus-x99-deluxe-ii.314207/) for his works, this project based on his EFI.
