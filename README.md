# dualbootmacbookpro2009
Debian 12 & Window 10 on MacbookPro 5.2 (2009)

You can find a feedback about how I achieve to install dual boot with Debian 12 and Windows 10 on an old Macbook Pro 5.2 17'' (2009), after a lot of fails.

Format your disk with MRB and not GPT, to force bios legacy install (for more information [https://www.antirandom.com/blog/2017/01/12/running-windows-10-on-an-older-macbook-pro/](https://www.antirandom.com/blog/2017/01/12/running-windows-10-on-an-older-macbook-pro/)

Start by finding a CD/DVD for a bios legacy install of Windows 10. 

Create a windows partition inside the Windows 10 installation tools. Be carrefful to let empty space on the disk to install Debian 12 after. Normally if the Windows is in legacy mode, there is no problem message about GPT or MBR problem at this stage.

You can install now or later bootcamp driver using Compatibility Mode Windows 2007. Nvidia drivers are ok, on the contrary to a EFI boot install of Windows 10, the necessite a lot a complicated tweek to make it works. 

Why bios legacy? because of it's the only (simple) way to install the boocamp driver, especially for NVidia 9600M GT (and on chip 9400M) driver using driver. There is a way to install using EFI but it's for experimented users (see there for details ).

Website for Bootcamp Windows 10 driver for Macbook Pro 5.2 : [https://www.driverscape.com/manufacturers/apple/laptops-desktops/macbookpro5%2C2/19724](https://www.driverscape.com/manufacturers/apple/laptops-desktops/macbookpro5%2C2/19724)

Install Debian 12 in expert mode, 

- use the install tools to create swap and / partition (eventually /home or others)
- do not force EFI Mode
- do not install bootloader grub. I recommand a light desktop manager of Debian, like XFCE

Install the BT4322 firmware debian package and its depedency (available here)

Install the compiled package dedicated for this specific computer
 - download the NVIDIA 340 Sebian package in /nvidia 
 - add this line in /etc/apt/sources.list
 - sudo apt update
 - sudo apt install nvidia-legacy-340xx-driver nvidia-settings-legacy-340xx
# If the above line doesn't work try with
# sudo apt install --no-install-recommends --no-install-suggests nvidia-legacy-340xx-driver nvidia-settings-legacy-340xx libgles1-nvidia-legacy-340xx libgles2-nvidia-legacy-340xx

Delete the previous video drivers, at least nouveau driver 
sudo apt autoremove xserver-xorg-video-nouveau

Create and edit with the following content X server configuration file /etc/X11/xorg.conf (that you can improve after) :

Section "Device"
    Identifier     "Device0"
    Driver         "nvidia"
    VendorName     "NVIDIA Corporation"
EndSection

Section "ServerFlags"
    Option         "IgnoreABI" "1"
EndSection



Normally when you reboot you should see the NVidia splash screen 



If you want to recompile the driver [(https://gist.github.com/Anakiev2/b828ed2972c04359d52a44e9e5cf2c63](https://gist.github.com/Anakiev2/b828ed2972c04359d52a44e9e5cf2c63)





