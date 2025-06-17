# 🚀 **Arch Linux + i3WM on ASUS VivoBook 2026**

**🛠️ System Deployment Report**
📅 **Date**: June 15, 2026
👨‍💻 **Senior System Administrator**: \[Sasha Zimin]
🧩 **System Version**: Arch Linux 2026.06.01

---

## 📚 Table of Contents

1. [🖥️ System Specifications](#️system-specifications)
2. [🕰️ Installation Timeline](#️installation-timeline)
3. [🔧 Detailed Installation Process](#️detailed-installation-process)
4. [✅ Post-Installation Verification](#️post-installation-verification)
5. [📦 Package Manifest](#️package-manifest)
6. [⚙️ System Configuration Summary](#️system-configuration-summary)

---

## 🖥️ System Specifications

| 🔍 Category        | 💡 Specification              |
| ------------------ | ----------------------------- |
| 🧠 Architecture    | `x86_64`                      |
| 🧭 Boot Mode       | `UEFI`                        |
| 💾 Filesystem      | `ext4` (root), `FAT32` (boot) |
| 🪟 Window Manager  | `i3-gaps (v4.23)`             |
| 🔐 Display Manager | `LightDM 1.30.0`              |
| 🌐 Network Manager | `iwd 2.11`                    |

---

## 🕰️ Installation Timeline

| ⏱️ Time     | 📌 Phase                        | ⏳ Duration |
| ----------- | ------------------------------- | ---------- |
| 15:00–15:10 | 🧰 Pre-installation Setup       | 10 min     |
| 15:10–15:15 | 💽 Disk Partitioning            | 5 min      |
| 15:15–15:30 | 📦 Base System Installation     | 15 min     |
| 15:30–15:45 | ⚙️ System Configuration         | 15 min     |
| 15:45–15:50 | 🔄 Bootloader Installation      | 5 min      |
| 15:50–15:55 | 👤 User Configuration           | 5 min      |
| 15:55–16:15 | 🖼️ Graphical Environment Setup | 20 min     |
| 16:15–16:20 | 🧹 Finalization                 | 5 min      |
| 16:20–16:35 | 🧪 Post-Installation            | 15 min     |

---

## 🔧 Detailed Installation Process

### 🧰 1. Pre-Installation (15:00)

```bash
# ✅ UEFI check
ls /sys/firmware/efi/efivars

# 🌐 Connect to Wi-Fi
iwctl --passphrase [WPA_KEY] station wlan0 connect [SSID]
ping -c 3 archlinux.org
```

### 💽 2. Disk Configuration (15:10)

```bash
# 🧱 Partitioning
parted /dev/nvme0n1 --script mklabel gpt
parted /dev/nvme0n1 --script mkpart ESP fat32 1MiB 513MiB
parted /dev/nvme0n1 --script set 1 boot on
parted /dev/nvme0n1 --script mkpart primary ext4 513MiB 100%

# 🧼 Filesystem creation
mkfs.fat -F32 -n EFI /dev/nvme0n1p1
mkfs.ext4 -L ROOT /dev/nvme0n1p2
```

### 📦 3. Base System Deployment (15:15)

```bash
pacstrap /mnt base linux linux-firmware base-devel
genfstab -U /mnt > /mnt/etc/fstab
```

### ⚙️ 4. System Configuration (15:30)

```bash
timedatectl set-timezone Europe/Kyiv
hwclock --systohc --utc

# 🌍 Locale setup
sed -i 's/#en_US.UTF-8/en_US.UTF-8/' /etc/locale.gen
locale-gen
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

### 🔄 5. Bootloader Setup (15:45)

```bash
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=ARCH
grub-mkconfig -o /boot/grub/grub.cfg
```

---

## 📦 Package Manifest

### 🔧 Core System

```bash
base linux linux-firmware grub efibootmgr sudo
```

### 🌐 Networking

```bash
iwd wpa_supplicant wireless_tools openssh
```

### 🖼️ Graphical Environment

```bash
xorg-server xorg-xinit i3-wm lightdm lightdm-gtk-greeter mesa-utils
glxinfo | grep "OpenGL"
```

### 🎮 For Intel Graphics

```bash
mesa lib32-mesa vulkan-intel glu freeglut glew
```

### 🧰 Utilities

```bash
htop ranger duf smartmontools xdg-utils brightnessctl
```

### 📝 Editors & File Management

```bash
vim nano mousepad pcmanfm mtpaint
```

### 🔤 Fonts

```bash
noto-fonts noto-fonts-emoji ttf-dejavu
```

---

## ✅ Post-Installation Verification (16:20)

### 📡 Service Status

```bash
systemctl is-active lightdm
systemctl is-enabled iwd
```

### 📦 Package Check

```bash
pacman -Q | grep -E 'i3|lightdm|noto'
```

### 🧱 Disk and Filesystem

```bash
df -hT
lsblk -f
```

---

## 🔈 Problem: No Sound (16:25)

### 🔍 Firmware Files Check

```bash
ls /usr/lib/firmware/intel/sof/sof-adl-n.ri
ls /usr/lib/firmware/intel/sof-tplg/sof-hda-generic-2ch.tplg
```

📦 *If missing, install:*

```bash
sudo pacman -S sof-firmware
```

🔄 *Then rebuild initramfs:*

```bash
sudo mkinitcpio -P
```

---

## ⚙️ System Configuration Summary

### 🗂️ Key Config Files

| File                        | Purpose                   |
| --------------------------- | ------------------------- |
| `/etc/i3/config`            | i3 WM layout and bindings |
| `/etc/lightdm/lightdm.conf` | DM setup                  |
| `/etc/iwd/main.conf`        | Network auto-connect      |
| `/etc/locale.conf`          | Language settings         |

### 🌐 Network Check

```bash
iwctl station wlan0 show
```

### 👤 User Privileges

```bash
sudo -l -U user
```

---

📄 **Report Generated**: June 15, 2026 – 16:40 EEST
✅ **System Ready for Deployment** – *All systems operational* ✔️

---
💻 `Arch + i3 = 💚`
