ArchLinux VPS at DigitalOcean (LEMP server)
===========================================
####How to use own kernels in DigitalOcean's droplet VPS with ArchLinux

#####You may read about problem with own kernels [here](https://www.digitalocean.com/community/articles/pacman-syu-kernel-update-solved-how-to-ignore-arch-kernel-upgrades) and [here](http://digitalocean.uservoice.com/forums/136585-digital-ocean/suggestions/2814988-give-option-to-use-the-droplet-s-own-bootloader-?page=1&per_page=20)


>#####@Sean commented  ·  June 10, 2013 4:29 p.m.
>#####This is how I set up my own kernels. (try at your own risk) http://www.youtube.com/watch?v=LHNPTvMwHPE



##Warning! Before you begin please make SNAPSHOT of you droplet in DigitalOcean control panel!
----------------------------------------------------------------------------------------------

I follow to Sean video (please see his youtube-video linked above) I made the trick with my Arch VPS.

There few steps after ssh to you droplet.

Switch to root (if you have sudo installed)

    sudo su

Install kexec-tools

    pacman -S kexec-tools -y

Remove systemd-sysvcompat

    pacman -R systemd-sysvcompat

Make you own boot script

    nano /tmp/init

And insert this code

    #!/bin/sh
		
    kexec --load /boot/vmlinuz-linux --initrd=/boot/initramfs-linux.img --append="root=LABEL=DOROOT init=/usr/lib/systemd/systemd" &&
    mount -o ro,remount / &&
    kexec -e

    exec /usr/lib/systemd/systemd

Change dir

    cd /sbin

Copy 'init' script

    cp /tmp/init ./

Make it executable

    chmod 0755 init

Go to pacman.conf and enable kernel updates

    nano /etc/pacman.conf

Change line 'IgnorePkg   =  linux'  - comment it  with #  (and save file!)

Then update system

    pacman -Syu

And after reboot you droplet ('sudo reboot' not work after remove systemd-sysvcompat)

    systemctl reboot

Check now you kernel  (uname -a) and happy!

-------------------------------------------------------------------------------------

Some usefull links about Kexec:

[Arch Wiki](https://wiki.archlinux.org/index.php/Kexec)

[IBM developerworks](http://www.ibm.com/developerworks/linux/library/l-kexec/index.html)
