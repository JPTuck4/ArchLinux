# ArchLinux Documentation

## 1: Verify Boot Mode
```
# ls /sys/firmware/efi/efivars
```

Since this command returned the directory, the system was booted in UEFI mode.

## 2: Check Default Network Config

```
# ping -c 4 www.linuxconfig.org
```

No Packets Lost

## 3: Update System Clock

```
# timedatectl set-ntp true
```

## 4: Partion The Disk

### Create Partitions

```
# lsblk
```

```
# cfdisk /dev/sda
```

Select gpt

Create first partition: 500M EFI System

Create second partition: 18.5G Linux filesystem

Create third partition: 1G Swap Linux Swap, write partition table to disk

Confirm Partions with ```lsblk```

### Create Filesystems

Swap:
```
# mkswap /dev/sda3
# swapon /dev/sda3
```

Root:
```
# mkfs.ext4 /dev/sda2
```

EFI:
```
mkfs.fat -F32 /dev/sda1
```

### Mount the partitions

Root:
```
# mount /dev/sda2 /mnt
```

EFI:

First create boot directory and then mount
```
# mkdir /mnt/boot
# mount /dev/sda1 /mnt/boot
```

## 5: Install Essential Packages

```
# pacstrap /mnt base linux linux-firmware
```

## 6: Generate fstab file
```
# genfstab -U /mnt >> /mnt/etc/fstab
```

## 7: Update timezone, localization and hostname
chroot into mnt:
```
# arch-chroot /mnt
```
Update timezone:
```
# ln -sf /usr/share/zoneinfo/US/Central /etc/localtime
```
Install vim:
```
# pacman -S vim
```
Edit /etc/locale.gen and uncomment en_US.UTF-8 UTF-8 using:
```
# vim /etc/locale.gen
```
Then generate locales with:
```
# locale-gen
```
Next add the line LANG=en_US.UTF-8 to the file /etc/locale.conf using:
```
# vim /etc/locale.conf
```
Edit the /etc/hostname and add chosen hostname jparchvm using:
```
# vim /etc/hostname
```
Finally, edit /etc/hosts to look like the following:
```
127.0.0.1   localhost
::1         localhost
127.0.1.1   jparchvm.localdomain  jparchvm
```



## 8: Networking
Enable the following:
```
# systemctl enable systemd-networkd
# systemctl enable systemd-resolved
```
Determine Network Interface using:
```
ip addr
```
Note: interface name is ens33

Edit /etc/systemd/network/20-wired.network and add the following:
```
[Match]
Name=ens33

[Network]
DHCP=yes
```

## 9: Set password
```
# passwd
```

## 10: Install Intel Microcode
```
# pacman -S intel-ucode
```

## 11: Install bootloader
Install grub and efibootmgr packages:
```
# pacman -S grub efibootmgr
```
Install grub bootloader to the EFI partition:
```
# grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
```
Generate main grub configuration file:
```
# grub-mkconfig -o /boot/grub/grub.cfg
```
Edit /etc/default/grub and add:
```
GRUB_DISABLE_OS_PROBER=false
```
Exit chroot
```
# exit
```

## 12: unmount partitions an d reboot system
```
# umount -R /mnt
# reboot
```
