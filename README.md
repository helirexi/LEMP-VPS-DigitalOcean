LEMP-VPS-DigitalOcean
=====================
How to use last kernels in Digital Ocean droplet 
---------------------------------------------------------------------------
@Sean commented  ·  June 10, 2013 4:29 p.m.
This is how I set up my own kernels. (try at your own risk) http://www.youtube.com/watch?v=LHNPTvMwHPE
--------------------------------------------------------------------------
Following to Sean video (see his post with youtube video) made this with my arch droplet - and very happy! 
Finally I have normal (not-Frankenstein) arch system as arch naturally is - always up-to-date! Big Thanks, Sean!

There few steps.

Switch to root (if you have sudo installed):
sudo su

Install kexec-tools:
pacman -S kexec-tools -y

Remove systemd-sysvcompat:
pacman -R systemd-sysvcompat

Make you own boot script:
nano /tmp/init
======code======================
#!/bin/sh

kexec --load /boot/vmlinuz-linux --initrd=/boot/initramfs-linux.img --append="root=LABEL=DOROOT init=/usr/lib/systemd/systemd" &&

mount -o ro,remount / &&

kexec -e

exec /usr/lib/systemd/systemd
======end==code========================

Change dir:
cd /sbin

Copy our script:
cp /tmp/init ./

Make it executable:
chmod 0755 init

Go to pacman.conf and enable kernel updates:
nano /etc/pacman.conf
=======================================
...
#IgnorePkg   =  linux
......
======================================
Change line 'IgnorePkg   =  linux'  - comment it  with #  (and save file!)

Then update system:
pacman -Syu

And after reboot you droplet ('sudo reboot' not work after remove systemd-sysvcompat):
systemctl reboot

Check now you kernel  (uname -a) and happy!
