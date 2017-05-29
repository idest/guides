Documenting my steps on Arch Linux
==================================

1. Network configuration
------------------------

#### Test network connection

    $ ping 8.8.8.8
If succesful press `ctrl + c` and skip to step 2

#### Check which kernel module contains the driver for your network device

    $ lspci -v

#### Check the driver was loaded

    $ dmesg | grep <kernel-module> (e.g. e1000e)

#### Get current device names

    $ ls /sys/class/net
  or
    $ ip link

#### Start dhcpcd daemon for a specific interface

    $ systemctl start dhcpcd@your_interface.service

#### Enable dhcpcd service (To be started on bootup)

    $ systemctl enable dhcpcd@your_interface.service

2. Add user
-----------

    $ useradd -m -G wheel -s /bin/bash your_username

Where:

`-m` creates the folder `/home/your_username`
`-G wheel` adds your_username to the weel group
`-s /bin/bash` defines bash as the user's default login shell

#### Set your password

    $ passwd your_username

3. Install and configure sudo
-----------------------------

#### Install sudo
    $ pacman -Syyu
    $ pacman -S sudo

#### Configure sudo

    $ EDITOR=nano visudo

#### Grant <username> sudo access

Uncomment the following line in `/etc/sudoers`:

    %wheel ALL=(ALL) ALL

4. Change login to your_username
--------------------------------

    $ exit

5. Create relevant recovery files
---------------------------------

#### Create recovery folder

    $ mkdir ~/recovery

#### List of all excplicitly installed packages

    $ pacman -Qqe > recovery/pkglist.txt

#### List of all installed optional dependencies

    $ comm -13 <(pacman -Qqdt | sort) <(pacman -Qqdtt | sort) > optdeplist.txt

#### List of all dependencies of base and base-devel

    $ comm -23 <(pacman -Qq | sort) <(pacman -Qgq base base-devel | sort) >> recovery/deps-base.txt

#### Backup of important directories

    $ cp /etc recovery/
    $ cp /var recovery/

#### Configuration files

    $ cp .* recovery/

##### Backup pacman database

    $ tar -cjf recovery/pacman_database.tar.bz2 /var/lib/pacman/local

#### Backup lists of files

    $ find /etc /opt /usr | sort > recovery/all_files.txt

    $ pacman -Qlq | sed 's|/$||' | sort > recovery/owned_files.txt

    $ comm -23 all_files.txt owned_files.txt > recovery/unowned_files.txt

6. Install base-devel group
---------------------------
(needed for building packages from AUR)

    $ pacman -Syu
    $ pacman -S --needed base-devel

7. Install git
--------------

    $ pacman -Syu
    $ pacman -S git

8. Install and configure zsh
-----------------------------

    $ pacman -Syu
    $ pacman -S zsh zsh-completions

#### Set zsh as the default shell

    $ chsh -s /bin/zsh

#### Install Oh my zsh framework

    sh -c "$(curl -fsSL https://raw.githubusercontent.com/robbyrussell/oh-my-zsh/master/tools/install.sh)"

#### Install patched powerline fonts (This is for the zsh agnoster theme)

    $ git clone https://github.com/powerline/fonts.git
    $ cd fonts
    $ ./install.sh
    $ cd ..
    $ rm -rf fonts

#### Set agnoster theme

Edit `.zshrc` and change/add the following lines:

    ZSH_THEME="agnoster"
    DEFAULT_USER="idest"

9. Install mesa-vdpau
---------------------

    $ pacman -Syu
    $ pacman -S mesa-vdpau

10. Install sway
---------------

    $ pacman -Syu
    $ pacman -S sway

#### Install the default software that sway is preconfigured to use

Install urxvt terminal emulator

    $ pacman -S rxvt-unicode

Install dmenu application launcher

    $ pacman -S dmenu

Install i3status status bar (this is not preconfigured but we will use it)

    $ pacman - S i3status

11. Configure sway
------------------

#### Copy the sample configuration file to your home and start editing it

    $ mkdir -p ~/.config/sway
    $ cp /etc/sway/config ~/.config/sway/
    $ nano ~/.config/sway/config

(various configuration files: http://dotshare.it/category/wms/i3/)
(sircmpwn configuration file: https://git.sr.ht/~sircmpwn/dotfiles/tree/.config/sway/config)

#### Add/change the following lines to `~/.config/sway/config`

Set the font:

    font pango:DejaVu Sans Mono 9

Set the display configuration (run `swaymsg -t get_outputs` on sway terminal) to check your display):

    output HDMI-A-2 resolution 1600x900 position 0,0

Set a wallpaper:

    output * bg ~/wallpaper.png stretch`
or
    output * bg ~/wallpaper.png fill

Set status bar:

    bar {
      status_command i3status
    }

Set the browser:

    set $browser chromium

Borderless windows:

    for_window [class="^.*"] border pixel 1
    new_window pixel 1
    new_float pixel 1

#### Set the keyboard layout

Create a script to run sway with the correct keyboard layout:

    $ mkdir ~/bin
    $ nano ~/bin/swayz

Add the following lines to `~/bin/swayz`:
(this code enables switching between the american and spanish layouts with alt + shift):

    #!/bin/bash
    export XKB_DEFAULT_LAYOUT=us,es
    export XKB_DEFAULT_VARIANT=,nodeadkeys
    esport XKB_DEFAULT_OPTIONS=grp:alt_shift_toggle,
    sway

Make the program `swayz` executable:

    $ chmod +x ~/bin/swayz

Add the following line to .zshrc:

    $ export PATH=~/bin:$PATH

Now everytime you want to run `sway` run `swayz` instead

> Keyboard repeat delay and rate

  Set the environment variables WLC_REPEAT_DELAY/WLC_REPEAT_RATE to the delay/rate in milliseconds before starting sway.

12. Set .Xdefaults
------------------

Add the following lines to ~/.Xdefaults (to set appareance of urxvt):
(more configs: https://bbs.archlinux.org/viewtopic.php?id=56605)

    ! urxvt

    URxvt*geometry:                53x20
    ! To add X11 fonts, do it like this:
    !   Rxvt*font:      -*-clean-medium-r-*--12-*-*-*-*-*-*-*
    ! To add xft fonts, do it like this:
    !   Rxvt*font:      xft:Bitstream Vera Sans Mono-8
    ! To list all fonts and styles known to fontconfig, run:
    !   $ fc-list : | sort
    ! To list all the X11 fonts run:
    !   $ fontsel
    ! To list all the xft fonts run:
    !   $ gtk2fontsel
    !   also you can look for them in /usr/share/fonts/TTF (true type fonts)
    !   (but apparently that folder does not contain ALL xft fonts in the system)
    ! Read more about fonts:
    !   browse the '/usr/share/doc/fontconfig/' docs
    !   man fc-list
    !   man urxvt
    URxvt*font:  xft:DejaVu Sans Mono for Powerline:pixelsize=12
    URxvt*depth:                32
    URxvt*background:            [85]#000000
    URxvt*foreground:            [100]#ffffff
    URxvt*borderless: 1
    URxvt*scrollBar:            false
    URxvt*saveLines:  5000
    URxvt*termName:    rxvt-unicode
    URxvt.perl-ext-common: default,matcher
    URxvt.urlLauncher: firefox
    URxvt.matcher.button: 1

    !Pnevma1
    *color0: #2F2E2D
    *color8: #4A4845
    *color1: #A36666
    *color9: #D78787
    *color2: #8FA57E
    *color10: #A9BA9C
    *color3: #D7AF87
    *color11: #E4C9AF
    *color4: #7FA5BD
    *color12: #A1BDCE
    *color5: #C79EC4
    *color13: #D7BEDA
    *color6: #8ADBB4
    *color14: #B1E7DD
    *color7: #D0D0D0
    *color15: #EFEFEF

    !Pnevma2 - A heavy mod of the original.
    !*color0: #2F2E2D
    !*color8: #4A4845
    !*color1: #C37561
    !*color9: #D19485
    !*color2: #A0A57E
    !*color10: #B6B99D
    !*color3: #D1A375
    !*color11: #DEBC9C
    !*color4: #7985A3
    !*color12: #98A1B9
    !*color5: #AB716D
    !*color13: #BE918E
    !*color6: #98B9B1
    !*color14: #CBE6CB
    !*color7: #D0D0D0
    !*color15: #EFEFEF


13. Install common applications
-------------------------------

#### Browser

    $ pacman -Syu
    $ pacman -S chromium
    # libx264, tff-DejaVu

14. Install and configure Uncomplicated Firewall and its GUI
------------------------------------------------------------

    $ pacman -Syu
    $ pacman -S ufw
    $ ufw default deny
    $ ufw allow from 192.168.0.0/24
    $ ufw allow SSH
    # To list all default applications rules supported (e.g. SSH)
      (based on common ports used by them)
    $ ufw app list
    # To list all rules added by the user
    $ ufw status
    # Enable the service
    $ ufw enable
    $ systemctl enable ufw.service
    # Install ufw GUI
    $ pacman -S gufw

15. Create builds and AUR directories
-------------------------------------

    $ mkdir ~/builds
    $ mkdir ~/builds/AUR

16. Install sublime-text-dev (Sublime Text 3 beta)
--------------------------------------------------

    $ cd ~/builds/AUR
    $ git clone https://aur.archlinux.org/sublime-text-dev.git
    $ cd sublime-text-dev
    # Always check files for malicious code
    $ less PKGBUILD
    $ less sublime-text-dev.install
    $ makepkg -si

17. Install nodejs npm for SublimeLinter
  > $ pacman -S nodejs npm

18. Install tidy for SublimeLinter-html-tidy
  > $ pacman -S tidy

19. Install pip and pip2 for SublimeLinter-pylint
  > $ pacman -S python-pip python-pip2
