# dualbootmacbookpro2009
Debian 12 & Window 10 on MacbookPro 5.2 (2009)

You can find a feedback about how I achieve to install dual boot with Debian 12 and Windows 10 on an old Macbook Pro 5.2 17'' (2009), after a lot of fails.

Format your disk with MRB and not GPT, to force bios legacy install.

Start by finding a CD/DVD for a bios legacy install of Windows 10. 

Create a windows partition inside the Windows 10 installation tools. Be carrefful to let empty space on the disk to install Debian 12 after. Normally if the Windows is in legacy mode, there is no problem message about GPT or MBR problem at this stage.

You can install now or later bootcamp driver using Compatibility Mode Windows 2007. Nvidia drivers are ok, on the contrary to a EFI boot install of Windows 10, the necessite a lot a complicated tweek to make it works. 

Why bios legacy? because of it's the only (simple) way to install the boocamp driver, especially for NVidia 9600M GT (and on chip 9400M) driver using driver. There is a way to install using EFI but it's for experimented users (see there for details ).

Website for Bootcamp Windows 10 driver for Macbook Pro 5.2 : [https://www.driverscape.com/manufacturers/apple/laptops-desktops/macbookpro5%2C2/19724](https://www.driverscape.com/manufacturers/apple/laptops-desktops/macbookpro5%2C2/19724)

Install Debian 12, in expert mode, do not force EFI Mode, do not install bootloader grub. I recommand a light desktop manager of Debian, like XFCE



