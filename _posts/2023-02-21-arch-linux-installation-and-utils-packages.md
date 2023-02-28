---
title: Arch Linux Installation + Utils Packages
author: robson
date: 2023-02-21 20:55:00 +0800
categories: [linux]
tags: [linux, archlinux]
pin: true
---

# Installation

## Set keyboard layout
```bash
loadkeys br-abnt2
```

## Connect to wifi
```bash
iwtcl
station wlan0 connect ""
```

## Partitioning & Formatting
> EFI System to /boot/EFI
{: .prompt-danger }
> Linux Filesystem to /
{: .prompt-danger }

```bash
cfdisk /dev/sdX
```

> FAT32 to /boot/EFI
{: .prompt-danger }
> EXT4 to / and /home
{: .prompt-danger }

```bash
mkfs.fat -F32 /dev/sdaX
mkfs.ext4 /dev/sdaX 
mkfs.ext4 /dev/sdaX 
```

## Mount file systems
```bash
mount /dev/sdaX /mnt && mount /dev/sdX /mnt/home
```

## Base Installation
```bash
reflector —-verbose —-lastest 3 —-sort rate --country Brazil —-save /etc/pacman.d/mirroslist
```
```bash
pacstrap /mnt base base-devel linux linux-firmware vim networkmanager sudo
```

## Install Bootloader
```bash
pacman -S grub efibootmgr dosfstools mtools os-prober
```

### UEFI
```bash
mkdir /boot/EFI &&
mount /dev/sdaX /boot/EFI &&
grub-install --target=x86_64-efi --bootloader-id=ARCH_GRUB --recheck &&
grub-mkconfig -o /boot/grub/grub.cfg
```

## Configure System
```bash
arch-chroot /mnt 
```
```bash
echo arch >> /etc/hostname
```

### Setup Locale
```bash
sed -i '/en_US.UTF-8 UTF-8/s/^#//g' /etc/locale.gen  &&
locale-gen &&
echo "LANG=en_US.UTF-8" > /etc/locale.conf
```

### Setup Keyboard
```bash
echo KEYMAP=br-abnt2 > /etc/vconsole.conf
```

### Configure pacman

> /etc/pacman.conf
{: .prompt-info }

```bash
[multilib]
Include = /etc/pacman.d/mirrorlist
```
```bash
ParallelDownloads = 5
```

## Setup users

### Set password root
```bash
passwd
```

### Add user
```bash
useradd -mg users -G wheel,storage,power -s /bin/bash robson
passwd robson
```

### Enable sudo
```bash
EDITOR=vim visudo
```
```bash
%wheel ALL=(ALL) ALL? %whel ALL=(ALL) NOPASSWD: ALL?
```

## Setup automount of partitions
```bash
genfstab -U /mnt >> /mnt/etc/fstab
genfstab -U /mnt/home >> /mnt/etc/fstab
```

## Enable NetworkManager Service
```bash
sudo systemctl enable NetworkManager
```

## Add keyring from Arch Reṕositories
```bash
pacman -S archlinux-keyring
```

---

# GUI

## GNOME
```bash
pacman -S gnome-session gdm xorg xorg-server-xwayland
```
```bash
echo "export XDG_SESSION_TYPE=x11
export GDK_BACKEND=x11
exec gnome-session" >> /home/$USER/.xinitrc
```
```bash
sudo systemctl enable gdm
```
---
```bash
pacman -S nautilus gnome-control-center gnome-system-monitor gnome-tweaks 
```
```bash
pacman -S gnome-themes-extra gthumb gnome-clocks gnome-calculator gnome-text-editor gnome-terminal 
```
```bash
pacman -S pacman-contrib xdg-user-dirs && xdg-user-dirs-update
```
---
- App Indicator - [https://extensions.gnome.org/extension/615/appindicator-support/](https://extensions.gnome.org/extension/615/appindicator-support/)
- Blur My Shell - [https://extensions.gnome.org/extension/3193/blur-my-shell/](https://extensions.gnome.org/extension/3193/blur-my-shell/)
- Dash to Dock - [https://extensions.gnome.org/extension/307/dash-to-dock/](https://extensions.gnome.org/extension/307/dash-to-dock/)
---

- BigSur Icon Theme - [https://www.gnome-look.org/p/1399044](https://www.gnome-look.org/p/1399044)

---

- Volume up - `F3`
- Volume down - `F2`
- Play (or play/pause) - `F5`

---
```bash
$ sed -i '/WaylandEnable=false/s/^#//g' /file-tal
```

## XFCE
```bash
sudo pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter xorg
```
```bash
sudo systemctl enable lightdm
```
---
```bash
sudo pacman -S dhcpcd && sudo systemctl enable dhcpcd
```


## SWAPFILE
```bash
dd if=/dev/zero of=/mnt/swapfile bs=1G count=1 &&
chmod 600 /mnt/swapfile &&
mkswap /mnt/swapfile &&
swapon /mnt/swapfile &&
echo "/mnt/swapfile swap swap defaults 0 0" >> /etc/fstab
```

# AUR
```bash
pacman -S --needed git base-devel &&
git clone https://aur.archlinux.org/yay.git &&
cd yay &&
makepkg -si
```

# AUR Packages
```bash
yay -S gnome-browser-connector firefox-extension-gnome-shell-integration gnome-terminal-transparency 
```

# PACKAGES
## UTILS
```bash
sudo pacman -S noto-fonts-emoji baobab gparted neofetch wget lsof
```

## EDITION
```bash
sudo pacman -S gimp kdenlive audacity handbrake breeze
```

## OTHERS
```bash
sudo pacman -S dconf-editor fuse-exfat ntfs-3g
```
```bash
sudo pacman -S transmission-qt obs-studio vlc telegram-desktop discord neofetch
```
```bash
yay -S spotify-launcher visual-studio-code-bin anki whitesur-icon-theme-git
```
```bash
pacman -S ffmpeg youtube-dl
```

# DOCK
- Nautilus
- Google Chrome
- VS Code
- Spotify
- Anki
- Clock
- Terminal
