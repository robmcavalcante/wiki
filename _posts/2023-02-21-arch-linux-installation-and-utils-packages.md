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

## connect to wifi
```bash
iwtcl
station wlan0 connect ""
```

```bash
# DISK PARTITIONING
cfdisk /dev/sdX # EFI System /boot/EFI, Linux Filesystem /

# FORMATING
mkfs.fat -F32 /dev/sdaX # /boot/EFI
mkfs.ext4 /dev/sdaX # /
mkfs.ext4 /dev/sdaX # /home
```

```
# ADD KEYRING FROM ARCH REPOSITORIES
pacman -S archlinux-keyring
```

```bash
# MOUNT /
mount /dev/sdaX /mnt

# CONFIGURING THE MIRRORLIST
reflector —-verbose —-lastest 3 —-sort rate --country Brazil —-save /etc/pacman.d/mirroslist

# uncomment parallel download on etc/pacman.conf
ParallelDownloads = 5

# INSTALLING THE SYSTEM BASE PACKAGES
pacstrap /mnt base base-devel linux linux-firmware vim networkmanager sudo

pacstrap /mnt base-devel
archlinux-keyring gcc grep sudo sed)
```

```bash
# MOUNT /HOME
mount /dev/sdX /mnt/home 
```

```bash
# CONFIGURING AUTOMATIC MOUNTING OF PARTITIONS
genfstab -U /mnt >> /mnt/etc/fstab
genfstab -U /mnt/home >> /mnt/etc/fstab
```

---

```bash
arch-chroot /mnt 

echo arch >> /etc/hostname

# SETTING THE LANGUAGE [uncomment "en_US.UTF-8 UTF-8"]
sed -i '/en_US.UTF-8 UTF-8/s/^#//g' /etc/locale.gen 
locale-gen
echo "LANG=en_US.UTF-8" >> /etc/locale.conf
```

```bash
# SETTING A PASSWORD FOR THE ROOT USER
passwd

# SETTING ... [uncomment "%whel ALL=(ALL) NOPASSWD: ALL"]
EDITOR=vim visudo
```

```bash
# ENABLE THE NETWORKMANAGER SERVICE
sudo systemctl enable NetworkManager
```

```bash
# CREATING A USER
useradd -mg users -G wheel,storage,power -s /bin/bash username
passwd username
```

```bash
# INSTALLING BOOT PACKAGES AND GRUB
pacman -S grub efibootmgr dosfstools mtools os-prober

# CREATING AND CONFIGURING THE SYSTEM BOOTLOADER
mkdir /boot/EFI
mount /dev/sdaX /boot/EFI
grub-install --target=x86_64-efi --bootloader-id=ARCH_GRUB --recheck
grub-mkconfig -o /boot/grub/grub.cfg
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
pacman -S nautilus gnome-control-center gnome-system-monitor gnome-tweaks gnome-browser-connector
```
```bash
pacman -S gnome-themes-extra gthumb gnome-clocks gnome-calculator gnome-text-editor gnome-terminal gnome-terminal-transparency 
```
```bash
pacman -S pacman-contrib xdg-user-dirs && xdg-user-dirs-update
```
---
- App Indicator - [https://extensions.gnome.org/extension/615/appindicator-support/](https://extensions.gnome.org/extension/615/appindicator-support/)
- Blur My Shell - [https://extensions.gnome.org/extension/3193/blur-my-shell/](https://extensions.gnome.org/extension/3193/blur-my-shell/)

---

- [https://www.gnome-look.org/p/1399044](https://www.gnome-look.org/p/1399044)

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
# SWAPFILE
```bash
dd if=/dev/zero of=/mnt/swapfile bs=1G count=1 &&
chmod 600 /mnt/swapfile &&
mkswap /mnt/swapfile &&
swapon /mnt/swapfile &&
echo "/mnt/swapfile swap swap defaults 0 0" >> /etc/fstab
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
