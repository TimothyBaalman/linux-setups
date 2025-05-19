# Hyprland with Nvidia
1. pacman -S nvidia nvidia-utils egl-wayland lib32-nvidia-utils xorg --ignore xorg-server-xdmx sddm sddm-runit hyprland git kitty packagekit-qt5 
2. accept defaults with enter unless you want something else
3. $ sudo vim /etc/mkinitcpio.conf
   1. add to modules: MODULES=(... nvidia nvidia_modeset nvidia_uvm nvidia_drm ...)
   2. $ :wq
   3. $ sudo mkinitcpio -P
   4. if you see missing firmware issues see [Missing Firmware for Modules](#missing-firmware-for-modules)
4. $ sudo vim ~/.config/hypr/hyprland.conf
   1. Add the following evnironment variables
   2. env = LIBVA_DRIVER_NAME,nvidia
   3. env = __GLX_VENDOR_LIBRARY_NAME,nvidia
5. $ ln -s /etc/runit/sv/sddm /run/runit/service
   1. Your sddm will start automatically after this
   2. to get into tty press alt+(one of the function keys mine was F4)F1-6
6. 

# Hyprland Installs
1. $ sudo pacman -S mako pipewire wireplumber xdg-desktop-portal-hyprland qt5-wayland qt6-wayland waybar wofi wayland-protocols dolphin hyprcursor hypridle hyprlang hyprutils hyprwayland-scanner hyprgraphics
2. add ```exec-once = waybar``` into ~/.config/hypr/hyprland.conf
3. $ git clone https://aur.archlinux.org/yay.git
   1. $ cd yay
   2. $ makepkg -si
4. $ sudo reboot
5. $ yay -S hyprlock hyprpaper hyprpicker hyprsunset
   1. only install the package that we have here
   2. if asked about dependency conflict press y to remove dependency

# Missing Firmware for Modules 
1. See [Possibly_missing_firmware_for_module_XXXX](https://wiki.archlinux.org/title/Mkinitcpio#Possibly_missing_firmware_for_module_XXXX)
2. $ sudo pacman -S linux-firmware-qlogic
3. $ yay -S ast-firmware wd719x-firmware aic94xx-firmware 
4. upd72020x-fw githud doesn't exist anymore
   1. [Arch upd72020x-fw](https://aur.archlinux.org/packages/upd72020x-fw)
   2. $ curl -O https://aur.archlinux.org/cgit/aur.git/snapshot/upd72020x-fw.tar.gz
      1. $ tar -xvzf upd72020x-fw.tar.gz
      2. $ cd upd72020x-fw
      3. $ less PKGBUILD
      4. does source link to the missing github? See [mahatmus-tech uPD72020x-Firmware](https://github.com/mahatmus-tech/uPD72020x-Firmware/tree/main) for refernce
      5. possible need [Arch remove hook ref](https://aur.archlinux.org/cgit/aur.git/tree/remove.hook?h=upd72020x-fw) too
   3. 

https://github.com/kurealnum/dotfiles/blob/main/.config/scripts/sysmaintenance.sh