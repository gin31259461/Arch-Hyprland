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

Adding flags to <u>**~/.config/electron-flags.conf**</u>

```.conf
--enable-wayland-ime
```

* Visual Studio Code

Adding flags to <u>**~/.config/code-flags.conf**</u>

```.conf
--enable-wayland-ime
```
