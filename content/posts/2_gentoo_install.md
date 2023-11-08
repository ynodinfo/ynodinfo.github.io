+++
title = "Gentoo Install"
author = ["Dony Ridani"]
date = 2023-07-02T00:00:00+08:00
lastmod = 2023-09-17T17:49:17+08:00
tags = ["gentoo", "linux"]
draft = false
+++

## I. Prepare Disk {#i-dot-prepare-disk}

```bash
#disk wipe
sgdisk --zap-all /dev/sda
#partition disk
cfdisk -z /dev/sda
mkfs.vfat -F 32 /dev/sda1
mkfs.btrfs/ext4/xfs/.... /dev/sda3
mkswap /dev/sda2
swapon /dev/sda2
#then mount them
```


## II. Installing stage3 {#ii-dot-installing-stage3}

Download the stage3 from Gentoo's website and put it in the /mnt/gentoo

```bash
# unpack it
tar xpvf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner
# get the portage config from my own git repo
# and set fstab
genfstab -U /mnt/gentoo >> /mnt/gentoo/etc/fstab
# then config repos.conf:
mkdir --parents /mnt/gentoo/etc/portage/repos.conf
cp /mnt/gentoo/usr/share/portage/config/repos.conf /mnt/gentoo/etc/portage/repos.conf/gentoo.conf
# Copy DNS info
cp --dereference /etc/resolv.conf /mnt/gentoo/etc/
# Mounting the necessary filesystems
mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --make-rslave /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev
mount --make-rslave /mnt/gentoo/dev
mount --bind /run /mnt/gentoo/run
mount --make-slave /mnt/gentoo/run
# chroot
chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"
# make sure all partition are mounted correct
emerge --sync
#set profle
eselect profile list
eselect profile set X
# config ccache before this step
emerge -auvDN --with-bdeps=y --autounmask-write @world
```

add ccache

```bash
emerge -av dev-util/ccache
#config ccache
mkdir -p /var/cache/ccache
chown root:portage -R /var/cache/ccache
chmod 2775 -R /var/cache/ccache
```

Then edit /var/cache/ccache/ccache.conf

```text
max_size = 512.0G
umask = 002
hash_dir = false
compiler_check = %compiler% -v
cache_dir_levels = 3
compression = true
compression_level = 1
```


## III. Post configure {#iii-dot-post-configure}

```bash
ln -sf ../usr/share/zoneinfo/Asia/Shanghai /etc/localtime
echo "en_US.UTF-8 UTF-8
zh_CN.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
eselect locale list
eselect locale set X
emerge -av sys-fs/btrfs-progs networkmanager app-admin/sysklogd sys-process/cronie sudo grub dev-vcs/git
systemctl enable NetworkManager
sed -i 's/\# \%wheel ALL=(ALL) ALL/\%wheel ALL=(ALL) ALL/g' /etc/sudoers
passwd root
# add GRUB_DISABLE_OS_PROBER=false to the end of /etc/default/grub
ln -sf /proc/self/mounts /etc/mtab
systemd-machine-id-setup
emerge -av sys-kernel/gentoo-kernel-bin
eselect kernel list
eselect kernel set X
# add use initramfs in linux-firmware
emerge -av genkernel linux-firmware
genkernel --install --compress-initramfs-type=zstd initramfs
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=Gentoo
grub-mkconfig -o /boot/grub/grub.cfg
emerge -av kde-plasma/plasma-meta xorg-server xf86-video-amdgpu media-fonts/noto
```
