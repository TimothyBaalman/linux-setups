# Hyprland with Nvidia
1. See the [Hyrpland Nvidia Doc](https://wiki.hyprland.org/Nvidia/)
2. $ sudo pacman -S nvidia nvidia-utils egl-wayland lib32-nvidia-utils xorg --ignore xorg-server-xdmx sddm sddm-runit hyprland git kitty packagekit-qt5 
3. accept defaults with enter unless you want something else
4. $ sudo vim /etc/mkinitcpio.conf
   1. add to modules: MODULES=(... nvidia nvidia_modeset nvidia_uvm nvidia_drm ...)
   2. $ :wq
   3. $ sudo mkinitcpio -P
   4. if you see missing firmware issues see [Missing Firmware for Modules](../base-install.md#missing-firmware-for-modules)
5. $ sudo vim ~/.config/hypr/hyprland.conf
   1. Add the following evnironment variables
   2. ```env = LIBVA_DRIVER_NAME,nvidia```
   3. ```env = __GLX_VENDOR_LIBRARY_NAME,nvidia```
   4. ```exec-once = pipewire```
   5. ```exec-once = pipewire-pulse```
6. $ sudo reboot
7. $ ln -s /etc/runit/sv/sddm /run/runit/service
   1. Your sddm will start automatically after this
   2. to get into tty press alt+(one of the function keys mine was F4)F1-6

# Hyprland Installs
1. $ sudo pacman -S mako pipewire wireplumber playerctl pipewire-pulse xdg-desktop-portal-hyprland qt5-wayland qt6-wayland waybar wofi wayland-protocols dolphin hyprcursor hypridle hyprlang hyprutils hyprwayland-scanner hyprgraphics
2. add ```exec-once = waybar``` into ~/.config/hypr/hyprland.conf
3. $ git clone https://aur.archlinux.org/yay.git
   1. $ cd yay
   2. $ makepkg -si
5. $ cp -ri /etc/xdg/waybar/ ~/.config/waybar/
6. Inside the config.jsonc
   1. replace sway/workspaces with hyprland/workspaces
   2. replace sway/mode with hyprland/submap
   3. replace sway/window with hyprland/window
   4. had issues with media module so removed it
7. $ sudo usermod -aG input \<username>
8. $ sudo reboot
9. $ yay -S hyprlock hyprpaper hyprpicker hyprsunset
   1. only install the package that we have here
   2. if asked about dependency conflict press y to remove dependency
10. add ```exec-once = hyprpaper``` into ~/.config/hypr/hyprland.conf
11. add ```exec-once = hyprsunset -d eDP-1 -t 4500``` into ~/.config/hypr/hyprland.conf
   1. Might make a bash script for all monitors
   ```sh
   #!/bin/bash
   for monitor in $(hyprctl monitors | grep 'Monitor' | awk '{print $2}'); do
      hyprsunset -d "$monitor" -t 4500 &
   done
   ```
   2. Save in ~/bin/hyprsunset-all
   3. $ chmod +x ~/bin/hyprsunset-all
   4. add ```exec-once = ~/bin/hyprsunset-all``` into ~/.config/hypr/hyprland.conf
   

https://github.com/kurealnum/dotfiles/blob/main/.config/scripts/sysmaintenance.sh