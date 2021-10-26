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

## 5:
