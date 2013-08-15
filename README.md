LEMP-VPS-DigitalOcean
=====================
###How to use last kernels in Digital Ocean droplet 

>@Sean commented  ·  June 10, 2013 4:29 p.m.
>This is how I set up my own kernels. (try at your own risk) http://www.youtube.com/watch?v=LHNPTvMwHPE

Following to Sean video (see his youtube video) made this with my arch droplet.

There few steps.

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
