<h1 align="center">Multi-Boot Configuration Using Clover !</h1>

## CLOVER

- Clover is an **open-source EFI-based** bootloader created on Apr 4, 2011. It has a totally different approach from Chameleon and Chimera. It can emulate the EFI portion present on real Macs and boot the OS from there instead of using the regular legacy **BIOS** approach used by Chameleon and Chimera.
  
- By using clover you can customize your boot menu in diffrent ways 

## Features

- [x] Supports for booting OS X, Windows and Linux (Android x86 as well)
- [x] Running on Legacy BIOS (MBR, GPT) or UEFI firmware (GPT only)
- [x] Customizable GUI including themes, icons, fonts, background images, animations, and mouse pointers.
- [x] ... and much more. [Here's](https://sourceforge.net/projects/cloverefiboot/) for details.

## What is Bootloader?

- A **bootloader**, also spelled as **boot loader** or called **boot manager** and **bootstrap loader**, is a computer program that is responsible for **booting a computer**. When a computer is turned off, its software‍—‌including operating systems, application code, and data‍—‌remains stored on **non-volatile memory**.

## Requirements

However, following this method you have to meet conditions below:

- Desktop PC or Laptop with UEFI **(GPT partition scheme is recommended)**
- Pre-installed Ubuntu Linux (and it's flavours), Debian or Live Mode
- Basic knowledge about BIOS (Firmware) configuration, partition scheme, OS Installation
- Make sure that your PC is able to boot Clover. For testing purpose; create USB Clover with this Tool via Windows. Then boot from it
- **Make a Backup of "Internal EFI Partition" on a safe place.**

## Before You Begin

- Clone or download the whole repo $ `https://github.com/avinashperera-ops/Clover.git`

- My Clover Themes collection `https://github.com/avinashperera-ops/MyCloverThemes.git`

## Configuration

- **Make sure that you have an UEFI partition style**

- How to check if your PC is using BIOS or UEFI on Windows 10? The most common way is to find it out by entering System Information window.

- 1. Open start menu in Windows 10.

- 2. Search for “System Information” and hit the “Result”.

- 3. Under the “System Summary” tab, spot “BIOS Mode”. If the value is “Legacy”, then, your computer is running on (Legacy) BIOS; if the value shows “UEFI”, it means your PC is running on UEFI boot mode.

## Manual Installation on GUID Partition Table (GPT)

Assummed "Target_Disk" is /dev/sda (Whole Disk) and "Target_Partition" is /dev/sda1 (EFI System Partition).
<br>Check with Terminal: `$ sudo blkid` or `sudo fdisk -l`<br/>

1. Run the command $ `sudo blkid`

2. Copy and paste the UUID of the relevant partitions in the relevant entries in the **config.plist** file

3. Placing Clover on EFI System Partition (ESP)
   <br>(Please note that `\EFI\BOOT` dir is not always empty, some linux distros maybe placing `grub, kernel, etc.` here. If this is your case, just copy `BOOTX64.efi` file, not replacing a whole dir).<br/>
 
	a. Option 1 via <b>Command Line</b>
	// <i>Mounting EFI System Partition</i><br/>
	- $ `mkdir ~/esp`
	- $ `sudo mount -t vfat /dev/sda1 ~/esp`
 
	// <i>Copying Clover required files</i>
	- $ `cd ~/CloverEFI-4MU`
	- $ `sudo cp boot ~/esp`
	- $ `sudo cp -r EFI/BOOT ~/esp/EFI`
	- $ `sudo cp -r EFI/CLOVER ~/esp/EFI`
 
	b. Option 2 via <b>File Manager</b> (GUI)
	- Mount ESP as Option (1) first
	- Terminal: $ `sudo [FileManager]` // File manager could be nautilus, pcmanfm, thunar, etc.
	- Manually copy-paste required files as point (a), be careful!
	- Terminal: $ `sudo umount ~/esp` (if all have done).

4. Then, Restart your PC and go to the BIOS.

5. After that, go to the boot options and in the boot sequence **(sometimes called as boot list)** add a new entry and select the clover file as the new entry.

6. Finally, save and exit the bios and ENJOY !!

## Bugs and Troubleshooting

- [ ] Press "F1" for Clover Help (Shortcut keys, Functions, etc.)
- [ ] Depends on your UEFI firmware, for adding CLOVER as "New Boot Entry" is usually:
   - `fsX:\efi\clover\cloverx64.efi` // (fsX = fs0, fs1, fs2, etc), or just
   - `\efi\clover\cloverx64.efi`

- [ ] Phoenix or InsydeH2O maybe not including "Boot" Entry Option on it's firmware (BIOS). On this case, you need manually adding Entry via UEFI Shell. For example adding Clover Entry located on FS0 (could be FS1, FS2 etc. depends on ESP):
	- `map FS*`
	- `bcfg boot dump`
	<br><i>// If `02` is last Boot Entry, add Clover on `03`:</i><br/>
	- `bcfg boot add 03 FS0:\EFI\CLOVER\CLOVERX64.efi "Clover EFI Bootloader"`
	<br><i>// OFC, you could add another entries eg. Windows Boot Manager, Linux Grub2, etc. on 04, 05..</i><br/>
	<br> If Clover is not set as 1st boot order, you need pressing StartUp key (could be F12, Esc, etc.) and manually select it once computer powered on. Most AMI Aptio BIOS has "Entry Override" option.<br/>

- [ ] If you're unable to "Launch EFI Shell" from BIOS, place "shellx64.efi or SHELLX64.EFI" on your ESP root (not EFI dir).
<br>Some old AMI Aptio firmwares (eg. v2.00) need this.<br/>

- [ ] Still having trouble accessing Shell via Firmware? Install Clover to Bootable USB FlashDisk using BDUtility.exe under Windows then boot from it. Run UEFI Shell provided by Clover.
