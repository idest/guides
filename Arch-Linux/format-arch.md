Steps to format Arch Linux system
---------------------------------

### Save any valuable files from the root partition into the home partition

### Boot from the Arch Linux installation media

### Identify your partitions

`$ lsblk`

`$ lsblk -f`

### Format the  root partition

`$ mkfs.ext4 root_partition` (e.g. root_partition = /dev/sda3)

### Mount the partitions

* Root partition:

`$ mount root_partition /mnt`

* Home partition:

    $ mkdir /mnt/home
    $ mount home_partition /mnt/home

* Boot partition:

    $ mkdir /mnt/boot
    $ mount boot_partition /mnt/boot

### Backup important home files and clean configuration files

    $ cd /mnt/home
    $ mkdir user_backup
    $ mv user/{dir1,dir2,file1,file2} /user_backup/
    $ rm -r user
    $ cd ~

### Select the fastest mirrors for your connection

    $ mv /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.backup
    $ rankmirrors -n 10 /etc/pacman.d/mirrorlist.backup > /etc/pacman.d/mirrorlist

### Remove old kernel files from boot partition

    $ rm /mnt/boot/{vmlinuz-linux,initramfs-linux.img,initramfs-linux-fallback-img}

### Install the base packages

    $ pacstrap /mnt base

### Generate an fstab file

    $ mkswap swap_partition
    $ swapon swap_partition
    $ genfstab -U /mnt >> /mnt/etc/fstab

### Change root into the new system

    $ arch-chroot /mnt

### Set the time zone and set the hardware clock:

    $ ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
    $ hwclock --systohc

### Change your locales

  Uncomment the line `en_US.UTF-8 UTF-8` in `/etc/locale.gen` and run:
    $ locale-gen

    $ echo "LANG=en_US.UTF-8" >> /etc/locale.conf

    $ echo "KEYMAP=your_keymap" >> /etc/vconsole.conf (e.g. your_keymap = es)

### Create a hostname

    $ echo "your_hostname" >> /etc/hostname

append the following line to `/etc/hosts`:

    127.0.1.1   your_hostanme.localdomain   your_hostname

### Set your password

    $ passwd

### Exit and reboot

    $ exit
    $ reboot
