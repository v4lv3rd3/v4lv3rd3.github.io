# v4lv3rd3.github.io

Arch Linux

Resources

⦁	ISO:  ⦁	Archlinux  (Torrent Mostly Recommended)
	
Virtual Machine: 
VirtualBox or ⦁	VMWare

Host :
Rufus or ⦁	balenaEtcher





Poner como Comprobar el checksum para evitar descargas malware

Installation

Power on The machine and Boot Arch

 
 


Temporal Keyboard configuration

– Spanish Keyboard: 
#loadkeys es 



Test the internet connection


# ping -c1 8.8.8.8 



Partition configuration 
⦁	VMachine : choose DOS
⦁	HOST : choose gpt


#cfdisk

 

select New and choose the first partition’s size , do it as much as partitions you want
if u dont know anything about partition types install ubuntu or debian, get more experience and try arch again
 

 
 

set swap :
when u have reach the limit space you'll see this screen
select the partition for swap and choose the type of partition
you must to select Linux Swap / Solaris (82)

 Linux Swap / Solaris (82)



 
Next you have to select write,  say yes and quit 



 # lsblk

 


Format partitions 

boot:
 # mkfs.vfat -F 32 /dev/sda1

system:
 # mkfs.ext4 /dev/sda2

swap:
 # mkswap /dev/sda3
 # swapon


Mount partitions

 # mount /dev/sda2 /mnt
 # mkdir -p /mnt/boot
 # mount /dev/sda1 /mnt/boot

This is the result :
 


basic Installation :

 # pacstrap /mnt linux linux-firmware networkmanager grub wpa_supplicant base base-devel


fstab generation


 # genfstab -U /mnt >/mnt/etc/fstab

Users and Root password


 # arch-chroot /mnt

Root password

 # passwd

User 
-m : if you want to create /home folder 

 # useradd -m -p password username


wheel group 
is like sudoers on debian 

Add user on this group

 # usermod -aG wheel username

Modify the sudoers file

 # pacman -S vim nano
 # nano /etc/sudoers

You have to uncomment the line  85
with nano you can see the line number :

 RightCtrl + 3 and RightAlt + 3 

 


set location


 # nano /etc/locale.gen 

uncomment the lines
 
 


 # locale-gen

set keymap permanent
create and write this content
 # nano /etc/vconsole.conf

KEYMAP=es


GRUB 


 # grub-install /dev/sda
 # grub-mkconfig -o /boot/grub/grub.cfg

set Hostname 

add these lines 

 # echo yourhostname >/etc/hostname
 # nano /etc/hosts

127.0.0.1      localhost
::1            localhost
127.0.0.1      yourhostname.localhost yourhostname
there is no spaces, use the tabulator , except between 
yourhostname.localhost yourhostname

Reboot
exit chroot 
if all is done , after reboot we have arch without GUI

 # exit 
 till u exit chroot 
 # reboot now





NetworkManager and wpa supplicant
if you have no internet :

 # systemctl start NetworkManager.service
 # systemctl enable NetworkManager.service

 # systemctl start wpa_supplicant.service
 # systemctl enable wpa_supplicant.service


AUR repos


 # pacman -S git

change to your system user


 # su username
 $ cd 
 $ mkdir -p Desktop/username/repos
 $ cd !$
!$ means last content separated by spaces of last command

 $ git clone https://aur.archlinux.org/paru-bin.git
 $ cd paru-bin
 $ makepkg -si


BlackArch repos


 $ cd ~/Desktop/username/repos
 $ mkdir blackarch
 $ cd !$
 $ curl -O https://blackarch.org/strap.sh
 $ chmod +x strap.sh
 $ sudo su
 # ./strap.sh


up to here the installation of arch without GUI



GUI Install and customization
Gnome

 # pacman -S xorg xorg-server gnome

let all default (means spam enter)

the default terminal dont works , IDK why , Install kitty
 # pacman -S alacritty
 # pacman -S gtkmm
 # systemctl enable gdm.service
 # systemctl start gdm.service

Keyboard  configuration gnome
  
 
 

QTILE
http://www.qtile.org/

 # pacman -S picom feh alacrity dmenu lightdm lightdm-gtk-greeter qtile     xorg xorg-server
 # nano /etc/lightdm/lightdm.conf

change this line
 
to 
 

 # systemctl enable lightdm.service 
 # reboot

Login With qtile interface 

Default Shortcuts qtile

Basic shortcut:
mod + <enter>: open terminal


 # setxkbmap es


VMWare tools (VMWare Only) 
 # pacman -S open-vm-tools
 # pacman -S xf86-video-vmware xf86-input-vmmouse
 # systemctl enable vmtoolsd
 # reboot now


