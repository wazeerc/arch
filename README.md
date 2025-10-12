# Arch Linux Installation Guide (Beginner-Friendly)

This guide will walk you through installing a barebones, functional Arch Linux system using **archinstall** with Hyprland as the primary window manager and KDE as a secondary desktop environment. Essential tools like git, vim, and Firefox will be pre-installed.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Creating a Bootable USB](#creating-a-bootable-usb)
3. [Booting from USB](#booting-from-usb)
4. [Pre-Installation Setup](#pre-installation-setup)
5. [Installing Arch Linux with archinstall](#installing-arch-linux-with-archinstall)
6. [Post-Installation](#post-installation)
7. [Troubleshooting](#troubleshooting)
8. [Additional Resources](#additional-resources)
9. [Setup & Customization](https://github.com/wazeerc/arch/blob/main/SETUP.md) ➕

---

## Prerequisites

Before you begin, ensure you have:

- **A USB drive** (4GB minimum, 8GB+ recommended)
- **A computer** with:
  - UEFI firmware (most modern systems)
  - At least 20GB of free disk space (50GB+ recommended)
  - Stable internet connection
- **Backup** of important data (installation will erase the target disk)
- **Basic familiarity** with command-line interface (CLI)

---

## Creating a Bootable USB

### Step 1: Download Arch Linux ISO

1. Visit the official Arch Linux download page: [https://archlinux.org/download/](https://archlinux.org/download/)
2. Download the latest ISO file (e.g., `archlinux-2024.xx.xx-x86_64.iso`)
3. Verify the ISO integrity by checking the checksums (recommended)

### Step 2: Create Bootable USB

#### On Linux:
```bash
# Find your USB device (e.g., /dev/sdb)
lsblk

# Write the ISO to USB (replace /dev/sdX with your USB device)
sudo dd bs=4M if=archlinux-2024.xx.xx-x86_64.iso of=/dev/sdX status=progress oflag=sync

# Sync to ensure all data is written
sync
```

#### On Windows:
1. Download **Rufus** from [https://rufus.ie/](https://rufus.ie/)
2. Open Rufus
3. Select your USB drive
4. Select the Arch Linux ISO file
5. Use **DD mode** when prompted
6. Click "Start" and wait for completion

#### On macOS:
```bash
# Find your USB device (e.g., /dev/disk2)
diskutil list

# Unmount the USB drive
diskutil unmountDisk /dev/diskX

# Write the ISO to USB
sudo dd if=archlinux-2024.xx.xx-x86_64.iso of=/dev/rdiskX bs=1m

# Eject the disk
diskutil eject /dev/diskX
```

---

## Booting from USB

### Step 1: Access BIOS/UEFI Settings

1. **Restart your computer**
2. **Press the boot menu key** during startup:
   - Common keys: `F2`, `F10`, `F12`, `Del`, or `Esc`
   - Check your motherboard/laptop manual for the specific key
3. **Disable Secure Boot** (if enabled):
   - Navigate to Security settings
   - Set Secure Boot to "Disabled"

### Step 2: Boot from USB

1. In the boot menu, select your USB drive
2. Select **"Arch Linux install medium"** from the boot menu
3. Wait for the live environment to load (you'll see a terminal prompt)

---

## Pre-Installation Setup

Once you've booted into the Arch Linux live environment, follow these steps:

### Step 1: Verify Boot Mode

Check if you're booting in UEFI mode:

```bash
ls /sys/firmware/efi/efivars
```

If the directory exists and shows files, you're in UEFI mode (recommended).

### Step 2: Set Keyboard Layout (Optional)

The default keyboard layout is US. If you need a different layout:

```bash
# List available layouts
ls /usr/share/kbd/keymaps/**/*.map.gz

# Load a specific layout (e.g., UK)
loadkeys uk
```

### Step 3: Connect to the Internet

**For Wired Connection:**
```bash
# Test connectivity
ping -c 3 archlinux.org
```

**For Wi-Fi Connection:**
```bash
# Start iwctl interactive prompt
iwctl

# Inside iwctl:
device list                              # List Wi-Fi devices (usually wlan0)
station wlan0 scan                       # Scan for networks
station wlan0 get-networks               # List available networks
station wlan0 connect "YourNetworkName"  # Connect (enter password when prompted)
exit                                     # Exit iwctl

# Verify connection
ping -c 3 archlinux.org
```

### Step 4: Update System Clock

```bash
timedatectl set-ntp true
timedatectl status
```

---

## Installing Arch Linux with archinstall

**archinstall** is the official guided installer that simplifies the installation process.

### Step 1: Launch archinstall

```bash
archinstall
```

You'll see a menu-driven interface. Follow the steps below:

---

### Step 2: Configure Installation Settings

Navigate through each menu option using arrow keys and Enter:

#### 1. **Archinstall language**
   - Select your preferred language (default: English)

#### 2. **Mirrors**
   - Select **"Mirror region"**
   - Choose your region/country for faster downloads
   - Multiple regions can be selected for redundancy

#### 3. **Locales**
   - **Locale language:** Select your language (e.g., `en_US`)
   - **Locale encoding:** Select `UTF-8` (default)

#### 4. **Disk configuration**
   - Select **"Use a best-effort default partition layout"**
   - Choose your target disk (e.g., `/dev/sda` or `/dev/nvme0n1`)
   - ⚠️ **Warning:** This will erase all data on the selected disk
   - **Filesystem:** Choose `ext4` (recommended for beginners) or `btrfs`
   - Confirm the changes

#### 5. **Disk encryption** (Optional)
   - Choose **"Encryption password"** if you want full disk encryption
   - Enter a strong password (you'll need this at every boot)
   - Or skip by leaving it blank

#### 6. **Bootloader**
   - Select **"Grub"** (recommended) or **"systemd-boot"**

#### 7. **Swap**
   - Select **"True"** to enable swap
   - This creates a swap partition for memory management

#### 8. **Hostname**
   - Enter a name for your computer (e.g., `arch-pc`)

#### 9. **Root password**
   - Enter a strong password for the root user
   - ⚠️ **Important:** Remember this password!

#### 10. **User account**
   - Select **"Add a user"**
   - **Username:** Enter your username (e.g., `yourusername`)
   - **Password:** Enter a strong password
   - **Super user (sudo):** Select **"yes"** to grant sudo privileges

#### 11. **Profile**
   - Select **"Desktop"**
   - Choose **"Hyprland"** as your desktop environment
   - When prompted for additional desktop environments:
     - Select **"KDE Plasma"** as well
   - This installs both Hyprland (primary) and KDE Plasma (secondary)

#### 12. **Audio**
   - Select **"Pipewire"** (modern audio system, recommended)

#### 13. **Kernels**
   - Select **"linux"** (stable kernel, recommended for beginners)
   - You can also add **"linux-lts"** for extra stability

#### 14. **Additional packages**
   - Enter the following packages separated by spaces:
     ```
     git vim firefox
     ```
   - These will be installed before first boot

#### 15. **Network configuration**
   - Select **"Use NetworkManager"** (recommended for desktop systems)
   - This provides easy network management with GUI tools

#### 16. **Timezone**
   - Select your timezone (e.g., `America/New_York`, `Europe/London`)

#### 17. **Automatic time sync (NTP)**
   - Select **"True"** to enable automatic time synchronization

---

### Step 3: Install

1. Review your configuration summary
2. Select **"Install"** to begin the installation
3. Confirm when prompted
4. Wait for the installation to complete (10-30 minutes depending on internet speed)

---

### Step 4: Complete Installation

Once installation finishes:

```bash
# Exit the installer if needed
# Remove the USB drive
# Reboot
reboot
```

---

## Post-Installation

### Step 1: First Boot

1. Your computer will restart
2. Remove the USB drive
3. You'll see the GRUB bootloader menu
4. Select **"Arch Linux"**
5. If you enabled disk encryption, enter your encryption password

### Step 2: Login

- **For Hyprland:** You'll see a login manager (SDDM or similar)
  - Enter your username and password
  - Select **"Hyprland"** from the session menu

- **For KDE Plasma:** 
  - At the login screen, click the session icon
  - Select **"Plasma (Wayland)"** or **"Plasma (X11)"**
  - Enter your username and password

### Step 3: Verify Installation

Open a terminal and verify essential packages:

```bash
# Check git
git --version

# Check vim
vim --version

# Check Firefox (GUI)
firefox --version
```

### Step 4: Update System

It's good practice to update your system after first boot:

```bash
sudo pacman -Syu
```

---

## Troubleshooting

### Issue: No Internet Connection After Installation

```bash
# Start NetworkManager
sudo systemctl start NetworkManager
sudo systemctl enable NetworkManager

# For Wi-Fi, use nmtui (text-based interface)
nmtui
```

### Issue: Can't Boot into Hyprland

```bash
# From login screen, try KDE instead
# Or from terminal:
startx  # For X11-based sessions
```

### Issue: Display Issues

```bash
# Install graphics drivers
# For NVIDIA:
sudo pacman -S nvidia nvidia-utils

# For AMD:
sudo pacman -S mesa xf86-video-amdgpu

# For Intel:
sudo pacman -S mesa xf86-video-intel

# Reboot after driver installation
sudo reboot
```

### Issue: Audio Not Working

```bash
# Check Pipewire status
systemctl --user status pipewire pipewire-pulse

# Start services if not running
systemctl --user start pipewire pipewire-pulse
systemctl --user enable pipewire pipewire-pulse
```

### Issue: Forgot Root Password

Boot from USB again and:
```bash
# Mount your root partition
mount /dev/sdXY /mnt  # Replace with your partition

# Chroot into your system
arch-chroot /mnt

# Reset root password
passwd root

# Exit and reboot
exit
reboot
```

---

## Additional Resources

### Official Documentation
- **Arch Wiki:** [https://wiki.archlinux.org/](https://wiki.archlinux.org/)
- **Installation Guide:** [https://wiki.archlinux.org/title/Installation_guide](https://wiki.archlinux.org/title/Installation_guide)
- **archinstall:** [https://wiki.archlinux.org/title/Archinstall](https://wiki.archlinux.org/title/Archinstall)

### Desktop Environments
- **Hyprland:** [https://hyprland.org/](https://hyprland.org/)
- **KDE Plasma:** [https://kde.org/plasma-desktop/](https://kde.org/plasma-desktop/)

### Package Management
- **Pacman Cheatsheet:** [https://wiki.archlinux.org/title/Pacman](https://wiki.archlinux.org/title/Pacman)
- **AUR (Arch User Repository):** [https://aur.archlinux.org/](https://aur.archlinux.org/)

### Community
- **Arch Linux Forums:** [https://bbs.archlinux.org/](https://bbs.archlinux.org/)
- **r/archlinux:** [https://reddit.com/r/archlinux](https://reddit.com/r/archlinux)

---

## Quick Reference Commands

```bash
# System update
sudo pacman -Syu

# Install packages
sudo pacman -S package_name

# Search for packages
pacman -Ss search_term

# Remove packages
sudo pacman -R package_name

# System info
neofetch          # Install with: sudo pacman -S neofetch

# Disk usage
df -h

# System logs
journalctl -xe
```

---

**Congratulations! You now have a functional Arch Linux system with Hyprland and KDE Plasma!** 🎉

Feel free to explore, customize, and enjoy your new Arch Linux installation. Remember: the Arch Wiki is your best friend for any additional configuration or troubleshooting.
