## Prepare for installation for Debian (draft)



### What are needed?

The following two ISOs will be needed.

- debian-live-9.5.0-i386-lxde.iso, https://cdimage.debian.org/debian-cd/current-live/i386/iso-hybrid/debian-live-9.5.0-i386-lxde.iso


### Prepare USB stick

**WARNING!!!**

> Please be informed that /dev/sda used as example. Please check the parameters according to your configuration

```sh
$ sudo dd if=debian-live-9.5.0-i386-lxde.iso of=/dev/sda bs=4M
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