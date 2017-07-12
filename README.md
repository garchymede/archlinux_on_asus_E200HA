# Archlinux on Asus E200HA

This is not about how to install archlinux but about how to get all the codecs and drivers so that you can enjoy this great tiny laptop. This is how i got everything working perfectly (sound, trackpad, keypad, full disk encryption, sd card, usb, backlight, function keys, etc),there could be other/better ways, but here you go.

The prerequities are that you have already installed archlinux on it, or that you can boot from the an installation media.The way I did it was to first install archlinux, and adding the new kernel on top of it before rebooting (otherwise there's no access to the keyboard if you encrypted the drive with luks...).


### 1. Building the kernel 

I recommend compiling the kernel from another computer since it should be faster than using the atom processor. If you cannot build it on another computer, step 2 won't be needed.

$ mkdir kernelbuild && cd ./kernelbuild

$ wget https://git.kernel.org/pub/scm/linux/kernel/git/tiwai/sound.git/commit/?h=topic/asus-e100h-4.12

$ tar xvf asus-e100h-4.12.tar.gz

$ cd ./asus-e100h-4.12

$ make clean && make mrproper

$ git clone https://github.com/garchymede/archlinux_on_asus_E200HA.git

  $ mv ./archlinux_on_asus_E200HA/.config_with_dm-cypt ./.config

<b>OR</b>

  $ mv ./archlinux_on_asus_E200HA/.config_without_dm-cypt ./.config

$ make xconfig

$ make


### 2. Sending all the files to the laptop. 


### 3. Installing the new kernel

Once in the new kernel directory, install the modules and the required files : 

$ make modules_install

$ cp -v arch/x86_64/boot/bzImage /boot/vmlinuz-vivobook

$ cp /etc/mkinitcpio.d/linux.preset /etc/mkinitcpio.d/vivobook.preset
and modify the file according to the name you chose. There are 3 changes to do :
- vmlinuz-linux => vmlinux-vivobook
- initramfs-linux.img => initramfs-linux.img
- initramfs-linux-fallback.img => initramfs-vivobook-fallback.img

$ mkinitcpio -p vivobook

$ grub-mkconfig -o /boot/grub/grub.cfg


### 4. Alsa config files

On the e200ha, get chtcx2072x.tar from this repository and move the config files to the right folder.

$ tar xvf chtcx2072x
$ sudo mv ./chtcx2072x/ /usr/share/alsa/ucm/

<b>reboot and enjoy</b>




