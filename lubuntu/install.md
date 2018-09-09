## Prepare for installation for Lubuntu (draft)



### What are needed?

The following two ISOs will be needed.

- lubuntu-18.04.1-desktop-amd64.iso, http://cdimage.ubuntu.com/lubuntu/releases/18.04.1/release/lubuntu-18.04.1-desktop-amd64.iso
- debian-live-9.5.0-i386-xfce.iso, https://cdimage.debian.org/debian-cd/current-live/i386/iso-hybrid/debian-live-9.5.0-i386-xfce.iso. Optional

### Prepare USB stick

**WARNING!!!**

> Please be informed that /dev/sda used as example. Please check the parameters according to your configuration

```sh
$ sudo dd if=lubuntu-18.04.1-desktop-amd64.iso of=/dev/sda bs=4M
$ sync
```

unplug and plug again USB stick

```
$ sudo fdisk /dev/sda
```

check disk configuration, it should be like that

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

The first partition with installation files, second one for EFI boot. In case if second partition does not exist, please create it as EFI (FAT-12/16/32) and format to FAT

```
$ sudo mkfs.fat -F32 /dev/sda2
```

The fist partition we will not touch, for second partition /dev/sda2 we will need to create the next structure and copy the next files from assets file `debian-9.5.0-boot.taz.bz2` https://github.com/ownport/lenovo-100s-11by-linux/releases/:

- /efi/boot/bootia32.efi
- /efi/boot/i386-efi/* 


### Boot from USB stick

```
grub> set prefix=(hd0,2)/efi/boot
grub> insmod linux
grub> insmod all_video
grub> linux /casper/vmlinuz file=/cdrom/preseed/lubuntu.seed boot=casper 
grub> initrd /casper/initrd.lz
grub> boot
```

## Installation Lubuntu

After Lubuntu Live started please follow the installation instructions

### TODO

- need to install `apt install grub-efi-ia32`