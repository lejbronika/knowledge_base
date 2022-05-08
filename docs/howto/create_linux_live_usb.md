# Create Linux Live-usb

## Download and verify linux `.iso`

- Download kali:
```
wget https://archive.kali.org/kali-images/kali-2022.1/kali-linux-2021.1-live-amd64.iso
```
- Download and import public GPG key:
```
wget -q -O - https://archive.kali.org/archive-key.asc | gpg --import

gpg --keyserver hkps://keys.openpgp.org --recv-key
```
- Extract fingerprint and get ISO SHASUMS:
```
gpg --fingerprint KEY_FROM_PREVIOUS_STEP

wget https://archive.kali.org/kali-images/kali-2022.1/SHA256SUMS

wget https://archive.kali.org/kali-images/kali-2022.1/SHA256SUMS.gpg
```
- Verify signature to ensure SHA256SUMS is authentic:
```
gpg --verify SHA256SUMS.gpg SHA256SUMS
```
Result (*ignore warnings*): `"Good signature from "Kali Linux Repository (devel@kali.org)"`
- Generate ISO SHASUM:
```
shasum -a 256 ./kali-linux-2022.1-live-amd64.iso
```
- Search for SHASUMS for downloaded version and compare them with generated on previous step:
```
grep kali-linux-2020.3-live-amd64 SHA256SUMS
```

## Create Live-usb

### Easy way -`dd`

- Find attached device with `dmesg`.
- Write ISO:
```
dd if=kali-linux-2022.1-live-amd64.iso of=/dev/sdb bs=1M status=progress
```
- Create and format additional partition. Verify with `lsblk`:
```
usb=/dev/sdb

fdisk $usb <<< $(printf "n\np\n\n\n\nw")
```

### Manual way

Based on this [stackexchange post](https://unix.stackexchange.com/questions/382817/uefi-bios-bootable-live-debian-stretch-amd64-with-persistence) and [Arch Linux Docs](https://wiki.archlinux.org/title/USB_flash_installation_medium#Using_manual_formatting).

- Unmount device:
```
umount /dev/sdX
``` 
- Create partitions with `parted`:  
	- EFI boot partition,
	- ISO partition with `legacy_boot` flag.
```
parted /dev/sdX --script mktable gpt
parted /dev/sdX --script mkpart EFI fat16 1MiB 10MiB
parted /dev/sdX --script mkpart live fat16 10MiB 3GiB
parted /dev/sdX --script mkpart persistence ext4 3GiB 100%
parted /dev/sdX --script set 1 msftdata on
parted /dev/sdX --script set 2 legacy_boot on
parted /dev/sdX --script set 2 msftdata on
```
- Create filesystems - FAT for EFI and live, ext4 for persistence:
```
mkfs.vfat -n EFI /dev/sdX1
mkfs.vfat -n LIVE /dev/sdX2
mkfs.ext4 -F -L persistence /dev/sdX3
```
- Mount the resources:
```
mkdir /tmp/usb-efi /tmp/usb-live /tmp/usb-persistence /tmp/live-iso

mount /dev/sdX1 /tmp/usb-efi
mount /dev/sdX2 /tmp/usb-live
mount /dev/sdX3 /tmp/usb-persistence
mount -oro live_img_name.iso /tmp/live-iso
```
- Install live system:
```
cp -ar /tmp/live-iso/* /tmp/usb-live
```
- [Add persistence](#add-persistence). 
- Grub for [UEFI](https://www.wisecleaner.com/think-tank/310-What-is-UEFI.html) support:
```
apt-get install grub2 grub-efi-amd64-bin
grub-install --no-uefi-secure-boot --removable --target=x86_64-efi --boot-directory=/tmp/usb-live/boot/ --efi-directory=/tmp/usb-efi /dev/sdX
```
- Syslinux for legacy BIOS support ([wiki.archlinux](https://wiki.archlinux.org/title/Syslinux#Installation_on_BIOS)):
```
apt-get install syslinux mtools
dd bs=440 count=1 conv=notrunc if=/usr/lib/syslinux/mbr/gptmbr.bin of=/dev/sdX
syslinux --install /dev/sdX2
```
- Reuse the `isolinux config` of original live ISO: 
```
mkdir /tmp/usb-live/syslinux
mv /tmp/usb-live/isolinux/* /tmp/usb-live/syslinux
mv /tmp/usb-live/syslinux/isolinux.bin /tmp/usb-live/syslinux/syslinux.bin
mv /tmp/usb-live/syslinux/isolinux.cfg /tmp/usb-live/syslinux/syslinux.cfg
```
- Modify Kernel parameters with [sed](https://www.unix.com/man-page/Linux/1/sed/):  
	- Add the `persistence` kernel parameter to `menu.cfg` and `grub.cfg`.
	- Set the `keyboard-layout` kernel parameter.
	- Fix the grub splash image (optional).
```
sed --in-place '0,/boot=live/{s/\(boot=live ,*\)$/\1 persistence/}' /tmp/usb-live/boot/grub/grub.cfg /tmp/usb-live/syslinux/menu.cfg

sed --in-place '0,/boot=live/{s/\(boot=live ,*\)$/\1 keyboard-layout=en locales=en_US.UTF-8,ru_RU.UTF-8/}' /tmp/usb-live/boot/grub/grub.cfg /tmp/usb-live/syslinux/menu.cfg

sed --in-place 's#isolinux/splash#syslinux/splash#' /tmp/usb-live/boot/grub/grub.cfg
```
- Unmount and cleanup:
```
umount /tmp/usb-efi /tmp/usb-live /tmp/usb-persistence /tmp/live-iso
rmdir /tmp/usb-efi /tmp/usb-live /tmp/usb-persistence /tmp/live-iso
```

### Why this should work for both UEFI and BIOS

> When starting in UEFI mode, the PC will scan the FAT partitions we defined in the GPT partition table. The first FAT partition carries the UEFI grub bootloader, which is found because it is located in the path specified by UEFI for removable drives (the --removable switch to grub-install did this). No UEFI boot entry is necessary for that to work, we only need to make the PC try to boot from the USB drive. That grub is configured to take it from there (load the grub.cfg, show the menu, etc.).  

> When starting in BIOS mode and selecting to boot from the USB drive, the PC will execute the gptmbr.bin bootloader code we have written to the protective MBR of the USB drive. That bootloader looks for the GPT partition marked with the legacy_boot flag and chainload syslinux from that partition. Syslinux then takes over (load menu.cfg, show the menu, etc.).

## Add persistence to Live-usb

- Create `ext3` or `ext4` file system in the partition and clabel ot `persistence`:
```
mkfs.ext3 -L persistence ${usb}3
```
- Create mount point, mount partition, create configuration file to enable persistence and unmount partition:
```
mkdir -p /mnt/usb_sd
mount ${usb}3 /mnt/usb_sd
echo "/ union" | sudo tee /mnt/usb_sd/persistense.conf
umount ${usb}3
```

## Add persistence with LUKS encryption to Live-usb

- Initialize LUKS encryption on created partition:

```
cryptsetup --verbose --verify-passphrase luksFormat ${usb}3
cryptsetup luksOpen ${usb}3 usb_sd
```
- Create `ext3` or `ext4` filesystem and label it `persistence`:
```
mkfs.ext3 -L persistence /dev/mapper/usb_sd
e2label /dev/mapper/usb_sd persistence
```
- Create mount point, mount encrypted partition, set up the `persistence.conf` and unmount partition:
```
mkdir -p /mnt/usb_sd/persistense
mount /dev/mapper/usb_sd /mnt/usb_sd
echo "/ union" | sudo tee /mnt/usb_sd/persistence.conf
umount /dev/mapper/usb_sd
```
- Close the encrypted channel to the partition:
```
cryptsetup lulksclose /dev/mapper/usb_sd
```

## Create new partition with `fdisk` <a name="fdisk-new-partition"></a>

- Select storage disk:
```
fdisk /dev/sdb
```
- Create new partition:  
	- `o` command to create partition table
	- `n` command to create new partition
	- select partition number
	- start sector: default
	- partition size: `+32GB`
	- `w` command to write on disk
- Format partition for necessary filesystem:
```
mkfs -t vfat /dev/sdb3
```

## Troubleshooting <a name="troubleshooting"></a>

- If messed up with partitions - just delete [device file](https://en.wikipedia.org/wiki/Device_file): `sudo rm /dev/sdb` and re-insert the device - [stackexchange](https://unix.stackexchange.com/questions/349052/dd-command-indicates-not-enough-disk-space-trying-to-format-sd-card-for-raspbe).
- Syslinux [manual install](https://wiki.archlinux.org/title/Syslinux#Manual_install).
