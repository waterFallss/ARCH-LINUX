# ARCH-LINUX
### Table of Contents
1. [Arch Linux Downloads and Pre-Installation Steps](#preinstall)
2. [Installation Steps on VMWare](#vmware)
3. [Check Connection with the Internet](#connection)

#### Arch Linux Downloads and Pre-Installation Steps <a name="preinstall"></a>
***
1. [Arch Linux Installation Guide](https://wiki.archlinux.org/title/Installation_guide)
2. Download ISO from your country: [ISO Download](https://archlinux.org/download/)
3. For this Walkthrough: [MIT ISO](http://mirrors.mit.edu/archlinux/iso/2023.10.14/)
4. Use ISO Image Signature for verification of file (Example: archlinux-2023.10.14-x86_64.iso.sig)
5. If gpg is not already installed: [GPG INSTALL](https://gpg4win.org/get-gpg4win.html)
```GPG Verification
 gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig   
```
#### Installation Steps on VMWare <a name="vmware"></a>
***
1. Click on File
2. Click on "New Virtual Machine"
3. Click on "Typical Configuration"
4. After selecting "Installer Disc Image File (iso)", find your iso file.
5. Guest Operating System: Linux
6. Version: Other Linux 5.x kernel 64-bit
7. Maximum DIsk Size: 20 GB (or Greater)
8. Store Virtual Disk as a Single File
9. Go to the setting on the Arch VM you created -> Click on "Options" -> "Advanced" -> Change Firmware type to UEFI
10. Power On
11. Click Enter on "Arch Linux Install Medium (x86_64, UEFI)"
12. Ensure that the system was booted in UEFI mode.
```
cat /sys/firmware/efi/fw_platform_size
```
#### Check Connection with the Internet <a name="connection"></a>
***
1. Check to see if your network interface is listed and enabled:
```
ip link
```
2. Ping archilinux.org for verification
```
ping archlinux.org
```
__Update System Clock__
***
1. Check the time zone.
```
timedatectl
```
2. If incorrect timezone, list out the time zones and set the it to the correct time zone.
```
timedatectl list-timezones
```
```
timedatectl set-timezone [TIME_ZONE]
```
__Partitioning__
***
1. Create EFI System Partition
```
fdisk /dev/sda
Command (m for help): n (create new Partition)
Partition type: p (primary partition)
Partition number: 1 (results in /dv/sda1)
First Sector: 2048 (default)
Last Sector: +500M
Command (m for help): t (Change partition type)
Hex Code or Alias: EF (Changes partition type from "Linux" to "EDI (FAT-12/16/32)"
```
2. Create Root Partition:
```
Command (m for help): n
Partition Type: p
Partition Number: 2
First Sector: [TYPE_DEFAULT_VALUE]
Last Sector: [VALUE_OF_CHOICE] (For this, +18G was added)
Command (m for help): w (Write table to disk and exit.)
```
__Formatting the Partitions__
***
1. For the Root Partition
```
mkfs.ext4 /dev/[ROOT_PARTITION]
Example: mkfs.ext4 /dev/sda2
```
2. For the EFI System Partition
```
mkfs.fat -F 32 /dev/[EFI_SYSTEM_PAARTITION]
Example: mkfs -F 32 /dev/sda1
```
__Mirror Selection__
***
1. Enable pacman to be able to download and install software
```
pacman -Syy
```
2. Install Refelector
```
pacman -S reflector
```
3. Create a Backup for the Mirror List
```
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
```
4. Obtain a mirror list from the reflector and save it.
```
reflector -c "US" -f 12 -l 10 -n 12 --save /etc/pacman.d/mirrorlist
```



