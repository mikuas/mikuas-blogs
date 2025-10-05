---
title: 如何安装ArchLinux
published: 2024-05-08
description: '本文章讲述了如何在电脑上安装ArchLinux操作系统'
image: ''
tags: ["Linux", "ArchLinux"]
category: 'Linux'
draft: false 
lang: ''
pinned: false
author: Mikuas
---

## 安装[ArchLinux](https://archlinux.org)

#### 校对时间
```bash
timedatectl set-ntp true
# 验证
date
```

:::WARNING[分区 查看磁盘号]
```bash
fdisk -l | lsblk

# 图形化分区(GPT)
cfdisk /dev/磁盘号

```
* 新建`EFI System` `Linux swap` `Linux filesystem` 分区并写入

#### 格式化磁盘
```bash
mkfs.ext4 /dev/Linux filesystem根目录分区
mkswap /dev/Linux swap交换分区
mkfs.fat -F 32 /dev/EFI System^EFI分区

# 挂载
mount /dev/Linux filesystem根目录分区 /mnt
mount --mkdir /dev/EFI System^EFI分区 /mnt/boot
swapont /dev/Linux swap交换分区
```
:::

### 更改改软件源
```bash
nano /etc/pacman.d/mirrorlist
add Server = https://mirrors.tuna.tsinghua.edu.cn/archlinux/$repo/os/$arch
```

### 重新安装密钥
```bash
pacman -Sy
pacman -S archlinux-keyring
```

:::NOTE[安装基本系统]
```bash
pacstrap -K /mnt base base-devel linux | [linux-zen] | linux-lts | ... linux-firmware linux-zen-headers

# 挂载信息
genfstab -U /mnt >> /mnt/etc/fstab
# 进入系统
arch-chroot /mnt
```
:::

### 设置时区
```bash
ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
hwclock --systohc
```

### 下载软件
```bash
networkmanager dhcpcd openssh git nano vim
```

### 设置语言
```bash
cd /etc
nano locale.gen     # 去掉en.US | zh.CN的注释
locale-gen          # 加载配置
nano locale.conf    # 编辑locale.conf 输入 LANG=en_US.UTF-

# 安装中文字体
pacman -S adobe-source-han-sans-cn-fonts
pacman -S ttf-dejavu wqy-zenhei wqy-microhei

```

### 设置`root`密码并添加用户
```bash
passwd

# 添加用户
useradd -m -G wheel username # 编辑visudo 去掉wheel的注释
```

### 编辑`hostname`文件
```bash
nano hostname

# 添加
127.0.0.1      localhost
::1            localhost
```

### 安装 `grub` `efibootmgr` 双系统安装 `os-prober`
```bash
cd/
nano /etc/default/grub # 去掉最后一行的注释

# UEFI引导
grub-install --target=x86_64-efi --efi-directory=boot --bootloader-id=GRUB

# 生成GRUB配置文件
grub-mkconfig -o /boot/grub/grub.cfg
```

### 编辑软件库
```bash
nano /etc/pacman.conf   #去掉multilib的注释 最下面添加:

[archlinuxcn]
SigLevel = Optional TrustAll
Server = https://mirrors.tuna.tsinghua.edu.cn/archlinuxcn/$arch
```

#### 导入密钥
```bash
pacman -Sy archlinuxcn-keyring
```

#### 安装`paru yay`
```bash
pacman -S paru yay
```

#### 安装`KDE`桌面
```bash
pacman -S xorg sddm plasma wayland konsole kate filelight dolphin ark sudo
# 设置sddm开机自启
```

#### 安装英伟达显卡驱动
```bash
pacman -S nvidia-dkms nvidia-utils linux-zen-headers

# 安装nvtop查看显卡使用率
pacman -S nvtop

# 编辑grub
nano /etc/default/grub    # 在GRUB_CMDLINE_LINUX_DEFAULT中加入nvidia_drm.modeset=1

# 更新
grub-mkconfig -o /boot/grub/grub.cfg

# 编辑`mkinitcpio`, 去掉`HOOKS`的 `kms`
nano /etc/mkinitcpio.conf

# 在MODULES中加入
nvidia nvidia_modeset nvidia_uvm nvidia_drm

# 重构
mkinitcpio -P linux-zen
```

#### 安装并配置输入法
```bash
# 安装中文输入法
pacman -S fcitx5-im fcitx5-chinese-addons fictx5-material-color

# 最下面添加,保存重启
GTK_IM_MODULE=fcitx
QT_IM-MODULE=fcitx
XMODIFIERS=@im=fcitx
SDL_IM_MODULE=fcitx
GLFW_IM_MODULE=ibus
```

---

#### 添加Windows启动引导
```bash
# 查看Windows引导分区的UUID
sudo fdisk -l

# 获取UUID
# sudo blkid /dev/disk_num

# 编辑/boot/grub/grub.cfg

menuentry 'Name' {
    insmod part_gpt
    insmod fat
    insmod chain
    search --fs-uuid --no-floppy --set=root UUID
    chainloader (${root})/EFI/Micorsoft/Boot/bootmgfw.efi
}
```

