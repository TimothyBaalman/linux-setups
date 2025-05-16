# Flash to a USB 

* https://download.artixlinux.org/iso/artix-base-runit-20250407-x86_64.iso

# Network Setup for wifi from USB

1. $ ip a # Do you see wlan0 for your wifi
2. $ ip link set wlan0 up
   1. if operation not possible due to RF-kill
   2. rfkill unblock wifi
   3. ip link set wlan0 up
3. $ connmanctl
   1. \> scan wifi
   2. \> services
   3. \> agent on 
   4. \> connect wifi_\<your-network-id> # Tab autocomplete works here
   5. \> enter password
   6. \> quit
4. $ ip a # Should now show an ip to your wlan0

# Partitioning

1. $ lsblk
2. $ cfdisk /dev/nvme0n1 # or your drive name if not nvme0n1
3. delete all partitions if you are wanting only to boot artix
   1. write 
   2. type yes
4. In free space press [new] making efi partition
   1. Give size as 1G
   2. change [type] to EFI System should be at the top
5. In free space press [new] making Linux Filesystem
   1. Hit enter for using the remainder of the drive
6. Press [write]
7. Type yes
8. [quit]
9. $ lsblk # to see your changes
10. $ mkfs.fat -F32 /dev/nvme0n1p1 # this is the efi partition
11. $ mkfs.ext4 /dev/nvme0n1p2 # This is the linux filesystem partition
   1.  type y if prompted
12. $ mount /dev/nvme0n1p2 /mnt
13. $ mkdir -p /mnt/boot/efi 
14. $ mount /dev/nvme0n1p1 /mnt/boot/efi
15. $ lsblk # should show the dirs on the drives now

# Base System Install
1. $ basestrap /mnt base base-devel runit elogind-runit linux linux-firmware vim intel-ucode linux-firmware-qlogic 
2. $ fstabgen -U /mnt >> /mnt/etc/fstab
3. $ cat /mnt/etc/fstab # should see two partitions here
4. $ artix-chroot /mnt # formally artools-chroot
5. $ dd if=/dev/zero of=/swapfile bs=1G count=2 status=progress # creating 2GB swapfile
6. chmod 600 /swapfile
7. mkswap /swapfile
8. swapon /swapfile
9. vim /etc/fstab 
   1.  type at the end ```/swapfile <\t>none<\t>swap<\t>defaults<\t>0 0```
   2.  save and exit :wq
10. $ ln -sf /usr/share/zoneinfo/America/Chicago /etc/localtime
11. $ hwclock systohc
12. $ vim /etc/locale.gen
    1.  look for en_uS.UTF-8 UTF-8 and uncomment(remove #)
    2.  :wq
13. $ locale-gen
14. $ vim /etc/locale.conf 
    1.  type LANG=en_uS.UTF-8
    2.  :wq
15. $ vim /etc/hostname
    1.  type name network name of machince artix-rice-mkr
    2.  :wq
16. $ vim /etc/hosts
    1.  type ```127.0.0.1<\t>localhost``` # for ipv4 address
    2.  type ```::1<\t><\t>localhost``` # for ipv6 address
    3.  type ```127.0.1.1<\t>artix-rice-mkr.localdomain<\t>artix-rice-mkr``` # for host name
17. $ passwd
    1.  enter password for root
18. $ pacman -S grub efibootmgr networkmanager networkmanger-runit network-manager-applet dosfstools linux-headers bluez bluez-runit bluez-utils cups cups-runit xdg-utils xdg-user-dirs dhcp dhcp-runit dbus dbus-runit
    1.  just press enter to accept the defaults if you don't care

# Grub bootloader Setup
1. $ grub-install --target=x86_64-efi --efi-directory=/boot/efi --bootloader-id=GRUB
   1. should see No error reported.
2. $ grub-mkconfig -o /boot/grub/grub.cfg
3. $ useradd -mG wheel \<user-name> # ex: timothy
4. $ passwd \<user-name>
   1. enter password
5. $ EDITOR=vim visudo
   1. uncomment %wheel ALL=(ALL) ALL
   2. :wq
6. $ exit
7. $ umount -R /mnt 
   1. target is busy is alright
8. $ reboot
9. On my system I had to add a new boot give it the name GRUB and select the /boot/efi folder

# Setup Runit System Softlinks
1. Login with creds
2. $ sudo su
3. $ ln -s /etc/runit/sv/NetworkManager /run/runit/service
4. $ ip a
5. for wifi $ nmtui 
   1. activate a connection
   2. select yours and enter password
   3. press esc
   4. $ ip a should now show an ip
6. $ ln -s /etc/runit/sv/bluetoothd /run/runit/service
7. $ ln -s /etc/runit/sv/cupsd /run/runit/service
8. $ ln -s /etc/runit/sv/dhcpd4 /run/runit/service
9. $ ln -s /etc/runit/sv/dhcpd6 /run/runit/service