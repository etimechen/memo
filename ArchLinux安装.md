#更新系统时钟

`timedatectl set-ntp true`

#硬盘分区格式化

fdisk /dev/sda
mkfs.ext4 /dev/sda1
mkswap /dev/sda3
swapon /dev/sda3

#挂载

mkdir -p /mnt/boot
mount /dev/sda1 /mnt/boot
mount /dev/sda2 /mnt

#安装base和base-devel

vim /etc/pacman.d/mirrorlist

pacstrap -i /mnt base base-devel

#生成fstab

genfstab -U /mnt >> /mnt/etc/fstab

#切换根目录

arch-chroot /mnt

#设置时区

ln -sf /usr/share/zoneinfo/Region/City /etc/localtime

hwclock --systohc

#地区
zh_CN.UTF8 UTF-8
en_US.UTF8 UTF-8
vim /etc/locale.gen

locale-gen

#编辑hostname

vim /etc/hostname      myhostname

vim /etc/hosts

127.0.0.1   localhost
::1     localhost
127.0.1.1   myhostname.localdomain  myhostname

#用户和密码

passwd

useradd -m -g users -s /bin/bash 用户名

passwd 用户名

vim /etc/sudoers

用户名 ALL=(ALL) ALL

#安装配置grub

pacman -S grub

grub-install --target=i386-pc /dev/sdX

grub-mkconfig -o /boot/grub/grub.cfg

#安装xorg

pacman -S xorg

#显示管理器

pacman -S lightdm lightdm-gtk-greeter

#安装桌面环境

pacman -S xfce4 xfce4-goodies gvfs networkmanager network-manager-applet


#开启服务

##网络服务

systemctl start dhcpcd.service

systemctl enable dhcpcd.service

##显示管理器服务

systemctl enable lightdm

pacman -S xf86-video-ati

##声卡

pacman -S alsa-utils xfce4-volumed


##输入法

.bashrc 里输入

```

export LC_CTYPE=zh_CN.UTF-8
export GTK_IM_MODULE=fcitx
export QT_IM_MODULE=fcitx
export XMODIFIERS="@im=fcitx"
eval `dbus-launch --sh-syntax --exit-with-session`
exec fcitx &

```

gvfs