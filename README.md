# Dual boot Windows 10 / Debian 12 sur Macbook Pro 5.2 17" (mi 2009)  

You can find a feedback about how I achieve to install dual boot with Debian 12 and Windows 10 on an old Macbook Pro 5.2 17'' (2009), after a lot of fails.

## Format your disk

Format your disk with MRB and not GPT, to force bios legacy install (for more information [https://www.antirandom.com/blog/2017/01/12/running-windows-10-on-an-older-macbook-pro/](https://www.antirandom.com/blog/2017/01/12/running-windows-10-on-an-older-macbook-pro/)).

Why bios legacy? because of it's the only (simple) way to install the boocamp driver after Windows 10 setup (without bootcamp neither MacOS), especially for NVidia 9600M GT driver. There is a way install it using EFI but it's for experimented users (see there for details [https://superuser.com/questions/671660/graphics-card-not-working-on-windows-8-1-on-mac](https://superuser.com/questions/671660/graphics-card-not-working-on-windows-8-1-on-mac).

## Install Windows

Start by finding a CD/DVD for a bios legacy install of Windows 10. 

Create a windows partition inside the Windows 10 installation tools. Be carrefful to let empty space on the disk to install Debian 12 after. Normally if the Windows is in legacy mode, there is no problem message about GPT or MBR problem at this stage.

You can install now or later bootcamp driver using Compatibility Mode Windows 2007. Nvidia drivers are ok, on the contrary to a EFI boot install of Windows 10, the necessite a lot a complicated tweek to make it works. 

Website for Bootcamp Windows 10 driver for Macbook Pro 5.2 : [https://www.driverscape.com/manufacturers/apple/laptops-desktops/macbookpro5%2C2/19724](https://www.driverscape.com/manufacturers/apple/laptops-desktops/macbookpro5%2C2/19724) or the excellent project Brigadier [https://github.com/timsutton/brigadier](https://github.com/timsutton/brigadier).

Check that Windows boot correctly using the ```alt``` or ```option``` key before starting.

## Create a boot loader rEFInd for secure boot before installing Debian 12

It's strongly advice to set up a USB key with et rEFIng boot loader [https://www.rodsbooks.com/refind/](https://www.rodsbooks.com/refind/) as secure boot, to be able to boot in case of erasing of windows boot loader or to correct grub boot loader mistakes on Debian 12.

## Install Debian 12 in expert mode

### Base installation

- use the install tools to create swap and ```/``` partition (eventually ```/home``` or others)
- DO NOT force EFI Mode
- you can avoid to install grub, anyway the computer will don't boot on windows before manually configuring grub2 under Debian 12 
- I recommand a light desktop manager of Debian, like XFCE

### Install the Wifi BT4322 firmware debian package and its depedency (available here)

Copy the files about the wifi driver BT4322 available in the repository in a directory, go to the directory, and : 
- ```sudo dpkg -i b43-fwcutter_019-14_amd64.deb```
- ```sudo dpkg -i firmware-b43-installer_019-14_all.deb```

If everything is ok the process ends by : 

```
...
Extracting b43/n0bsinitvals17.fw
Extracting b43/a0g1initvals5.fw
Extracting b43/n0initvals24.fw
Extracting b43/lp1initvals20.fw
Extracting b43/a0g0bsinitvals5.fw
Extracting b43/ucode9.fw
Extracting b43/ucode5.fw
Extracting b43/ucode22_mimo.fw
```
If you reboot you should see Wifi network somewhere in your desktop manager.

### Install grub2

I STRONGLY RECOMMAND TO HAVE A rEFInd USB AND CHECK IT WORKS BEFORE INSTALLING GRUB2 BOOT LOADER. 

For installing grub, remove all version of grub, and install grub2.

- ```sudo apt remove grub``
- ```sudo apt install grub2``

Configure your grub to boot Windows 10 and Debian 12 and other things if you want in the ```/boot/grub/grub.conf```

YOU HAVE TO ADAPT TO YOUR CONFIGURATION. For me I've one disk (hd0) and the 1st partition is the windows boot pertion (second entry of the nex example).
Debian is set up for me on the 3rd partition (the partition where is mounted ```/```). 
```
menuentry 'Debian GNU/Linux, kernel 2.6.26-2-amd64' {
  set root='(hd0,3)'; set legacy_hdbias='0'
  legacy_kernel   '/boot/vmlinuz-6.1.0-37-amd64' '/boot/vmlinuz-6.1.0-37-amd64' 'root=/dev/sda3' 'ro' 'quiet'
  legacy_initrd '/boot/initrd.img-6.1.0-37-amd64' '/boot/initrd.img-6.1.0-37-amd64'
}

menuentry 'Microsoft Windows 10 Professionnel' {
  set root='(hd0,1)'; set legacy_hdbias='0'
  chainloader  '+1'
}
```

### Install the NVidia 340 firmware debian package (nvidia-legacy-340...) and its depedency (available here) for this specific computer
 - download the NVIDIA 340 Debian compiled packages in /nvidia (available in repository)
 - add this line in ```deb [trusted=yes] file:/nvidia ./``` in ```/etc/apt/sources.list```
 - check that contrib packages are (the work contrib appear a the end of lines of your ```/etc/apt/sources.list``` ) 
 - ```sudo apt update```
 - ```sudo apt install nvidia-legacy-340xx-driver nvidia-settings-legacy-340xx```
 - 
If the above line doesn't work try with
```sudo apt install --no-install-recommends --no-install-suggests nvidia-legacy-340xx-driver nvidia-settings-legacy-340xx libgles1-nvidia-legacy-340xx libgles2-nvidia-legacy-340xx```

Delete the previous video drivers, at least nouveau driver 
```sudo apt autoremove xserver-xorg-video-nouveau```

If you want to recompile the driver [(https://gist.github.com/Anakiev2/b828ed2972c04359d52a44e9e5cf2c63](https://gist.github.com/Anakiev2/b828ed2972c04359d52a44e9e5cf2c63)

### Install the X server

Create and edit with the following content X server configuration file ```/etc/X11/xorg.conf``` (that you can improve after) :
```
Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
EndSection

Section "ServerFlags"
    Option         "IgnoreABI" "1"
EndSection
```
Normally when you reboot you should see the NVidia splash screen 






