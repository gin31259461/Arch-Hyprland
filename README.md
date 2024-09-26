# Installation Guide for Arch Linux with Hyprland

[TOC]

## Install Arch Linux and Hyprland

1. [Linutil](https://github.com/ChrisTitusTech/linutil)
Installation script for arch linux.

2. [JaKooLit's Arch-Hyprland](https://github.com/JaKooLit/Arch-Hyprland)
Hyprland install script and configuration.

## VMware

### Note

* enable vmware to pass battery information to guest devices.

### Enable extra mouse

Add the following configuration to vmx file (make sure the vm is power off).

```vmx
usb.generic.allowHID = "TRUE"
mouse.vusb.enable = "TRUE"
```

### Fix startup stuttering

Add the following configuration to vmx file (make sure the vm is power off).

```vmx
sound.highPriority = "TRUE"
```

Other sound problem refer to following link

- [Audio/Videao stuttering/crackling, Firefox + PipeWire in VMs](https://bbs.archlinux.org/viewtopic.php?id=280654)
- [Pipewire: Stuttering Audio (in Virtual Machine)](https://gitlab.freedesktop.org/pipewire/pipewire/-/wikis/Troubleshooting#stuttering-audio-in-virtual-machine)

## Hyprland

### Fcitx5 (Chinese input)

#### Setup

Run following command to install necessary packages.

```bash
paru -S fcitx5-im fcitx5-chewing
```

Using hyprland to autostart fcitx5 on boot.

```.conf
exec-once = fcitx5 &
```

Adding the following setting to hyprland environment to enable fcitx5 to use.

```.conf
env = GTK_IM_MODULE,fcitx
env = QT_IM_MODULE,fcitx
env = XMODIFIERS,@im=fcitx
env = GLFW_IM_MODULE,fcitx
env = INPUT_METHOD,fcitx
env = IMSETTINGS_MODULE,fcitx
env = SDL_IM_MODULE,fcitx
```

#### Flags

To enable fcitx5 used in third party apps, such as electron based apps, we need to add flags.

* Electron

Adding flags to **~/.config/electron-flags.conf**

```.conf
--enable-wayland-ime
```

* Visual Studio Code

Adding flags to **~/.config/code-flags.conf**

```.conf
--enable-wayland-ime
```

### [Clipboard manager](https://wiki.hyprland.org/Useful-Utilities/Clipboard-Managers/)

Using `cliphist` as clipboard manager.

Start by adding the following lines to hyprland config

```.conf
exec-once = wl-paste --type text --watch cliphist store # Stores only text data
exec-once = wl-paste --type image --watch cliphist store # Stores only image data
```

To bind `cliphist` to a hotkey for rofi

```.conf
bind = SUPER, V, exec, cliphist list | rofi -dmenu | cliphist decode | wl-copy
```

### Remote Desktop using VNC (wayvnc)

- [wayvnc](https://github.com/any1/wayvnc)

1) Install `wayvnc` from AUR

```bash
paru -S wayvnc
```

2) Encryption & Authentication (RSA-AES) 

```bash
mkdir ~/.config/wayvnc

ssh-keygen -m pem -f ~/.config/wayvnc/rsa_key.pem -t rsa -N ""

nvim ~/.config/wayvnc/config
```

3) Setting parameters

```.conf
use_relative_paths=true
address=0.0.0.0
enable_auth=true
username=user
password=****
rsa_private_key_file=rsa_key.pem
```

4) Finally, setting autostart on booting

```.conf
exec-once = wayvnc 127.0.0.1 5900 &
```

5) Now we can access hyprland using vnc viewer
