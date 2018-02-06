# Getting Linux onto the Lenovo 100s

Guides for installation linux on Lenovo 100s 11by


## Hardware configuration

- http://www3.lenovo.com/us/en/laptops/ideapad/ideapad-100-series/IdeaPad-100S-11IBY/p/88EM10S0639
- http://www3.lenovo.com/hk/en/laptops/ideapad/ideapad-100-series/IdeaPad-100S-11IBY/p/88EM10S0639#tab-tech_specs

| Key           | Value            |
| ------------- | ---------------- |
| Processors    | Intel® Atom™ Z3735F Quad-Core Processor   
| Graphics      | Integrated Intel® HD             
| Memory        | Up to 2 GB DDR3L
| Webcam        | 0.3MP with Dual Microphone
| Storage       | Up to 64 GB eMMC
| Audio         | Stereo Speakers with Dolby® Advanced Audio™ Certification
| Battery       | Up to 8 Hours at 150 nits Web Browsing
| Display       | 11.6" HD LED Glossy (1366x768) with integrated camera
| Dimensions (W x D x H) |     (inches) : 11.5" x 7.95" x 0.69", (mm) : 292 x 202 x 17.5
| Weight        | Starting at 2.2 lbs (1 kg)
| WiFi and Bluetooth® | WiFi 1 x 1 802.11 b/g/n, Bluetooth® 4.0
| Connectors    | 2 x USB 2.0, HDMI-out, microSD card slot, audio combo jack

## Prepare for installation (draft)

**WARNING!!!** Please check the parameters according to your configuration


### What are needed?

The following two ISOs will be needed.

- debian-live-9.1.0-i386-xfce.iso 
- linuxmint-18.2-xfce-32bit.iso


### Prepare USB stick

```sh
$ sudo dd if=linuxmint-18.2-xfce-32bit.iso of=/dev/sdX bs=4M
$ sync
```
unplug and plug again USB stick

```
$ sudo fdisk /dev/sda
```
make new partition with EFI
```
Command (m for help): p
Disk /dev/sda: 14.7 GiB, 15728640000 bytes, 30720000 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x49b13675

Device     Boot Start     End Sectors  Size Id Type
/dev/sda1  *        0 2566143 2566144  1.2G 17 Hidden HPFS/NTFS
```
You can use the "n" command in fdisk to create a partition.

```

Command (m for help): p
Disk /dev/sda: 14.7 GiB, 15728640000 bytes, 30720000 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0x49b13675

Device     Boot   Start      End  Sectors  Size Id Type
/dev/sda1  *          0  2566143  2566144  1.2G 17 Hidden HPFS/NTFS
/dev/sda2       2566144 30719999 28153856 13.4G ef EFI (FAT-12/16/32)
```

```
$ sudo mkfs.fat -F32 /dev/sda2
```

The structure of /dev/sda2
- /efi/boot/bootia32.efi (from Debian 32 bit installation)
- /efi/boot/i386-efi/* (from Debian 32 bit installation)

- copy all directories and files from the Mint ISO (other than the boot directory) to your sdX2 partition


### Boot from USB stick

```
grub> ser prefix=(hd0,2)/efi/boot
grub> insmod linux
grub> insmod all_video
grub> linux /casper/vmlinuz file=/cdrom/preseed/linuxmint.seed boot=casper 
grub> initrd /casper/initrd.lz
grub> boot
```

## Installation Linux Mint

TODO need to install `apt install grub-efi-ia32`

