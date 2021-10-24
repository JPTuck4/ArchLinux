# ArchLinux Documentation

## 1: Verify Boot Mode
```# ls /sys/firmware/efi/efivars```

Since this command returned the directory, the system was booted in UEFI mode.

## 2: Connect to the Internet
**Make sure the card is not blocked**

```rfkill list```

**Install iwd:**

```pacman -Sy iwd```

**Start and Enable iwd.service**

```systemctl start iwd.service```

```systemctl enable iwd.service```

### Connect to wireless:
```iwctl```

See device name: ```device list```
-No devices are showing
