# PatchBoot
This repository provides a guide for patching [AMI Aptio V UEFI firmware](https://www.ami.com/bios-uefi-utilities/) to circumvent [Secure Boot](https://learn.microsoft.com/en-us/windows-hardware/design/device-experiences/oem-secure-boot) checks and allow loading of unsigned executables.

## Disclamer
Flashing modified firmware onto your motherboard carries the risk of damaging it. On certain motherboards, **this damage could be irreparable**. Please proceed with caution - you have been warned.

## Reasons for patching
I use a single computer for both gaming and development, and my daily drivers are Arch Linux (yes, I felt the need to mention it) and Windows 10 LTSC/Windows 11. The issue is that certain intrusive anti-cheat systems require Secure Boot to be enabled. Though dual-booting with Secure Boot on is technically possible, it's an incredibly tedious process (and I would need to use different bootloader). So, I decided to straight up patch the firmware. I've [previously attempted simpler solutions](https://github.com/SamuelTulach/SecureFakePkg), but these anti-cheat systems have already adapted to detect and block such methods, preventing me from playing.

## Guide
Before starting with the entire process, ensure there is a way to flash modded firmware onto your motherboard. I have [ASUS ROG Strix X570-E Gaming](https://rog.asus.com/motherboards/rog-strix/rog-strix-x570-e-gaming-model) which has [USB BIOS FlashBack](https://www.asus.com/us/support/FAQ/1038568/) functionality that conveniently does not validate provided files. Other motherboards should be able to use [AMI AFU](https://www.ami.com/bios-uefi-utilities/). You can always search the internet for motherboard name + modded BIOS to see if there are any guides on flashing it and what tools are needed.

### Getting the original firmware
While it's possible to directly dump the firmware from your motherboard, in most cases, you can simply use the update .CAP file provided by your motherboard manufacturer. In my case, I simply downloaded [the latest one here](https://rog.asus.com/motherboards/rog-strix/rog-strix-x570-e-gaming-model/helpdesk_bios/).

![screenshot](assets/0_download.png)

### Extracting EFI binaries
Once you have the firmware file, open it in [UefiTool](https://github.com/LongSoft/UEFITool) (make sure to use the latest stable version, as the nightly rewrite does not support module replacing).  

![screenshot](assets/1_uefitool.png)

After loading the file, navigate to 'File > Search', and execute a case-insensitive text search for the string 'image verification'.

![screenshot](assets/2_search.png)

## Detection vectors