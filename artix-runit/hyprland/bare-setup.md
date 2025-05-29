# Hyprland with Nvidia
1. See the [Hyrpland Nvidia Doc](https://wiki.hyprland.org/Nvidia/)
2. $ sudo pacman -S nvidia nvidia-utils egl-wayland wayland-protocols  lib32-nvidia-utils xorg --ignore xorg-server-xdmx sddm sddm-runit hyprland git kitty packagekit-qt5 
3. accept defaults with enter unless you want something else
4. $ sudo vim /etc/mkinitcpio.conf
   1. add to modules: ```MODULES=(... nvidia nvidia_modeset nvidia_uvm nvidia_drm ...)```
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
1. [Check out Useful Utilities](https://wiki.hyprland.org/Useful-Utilities/)
   1. [Also look at Awesome Hyprland](https://github.com/hyprland-community/awesome-hyprland)
2. $ sudo pacman -S 
   1. ```mako``` # notification daemon 
   2. ```pipewire wireplumber playerctl pipewire-pulse``` # needed for screensharing and audio replacement
   3. ```xdg-desktop-portal-hyprland``` # lets other applications communicate with the compositor through D-Bus
   4. ```qt5-wayland qt6-wayland``` # Qt Wayland Support
   5. ```waybar``` # Status bar
   6. ```wofi``` # App launcher [docs](https://cloudninja.pw/docs/wofi.html)
   7. ```dolphin``` # File Manager [docs](https://github.com/KDE/dolphin)
   8. ```hyprcursor``` # is a new cursor theme format
   9. ```hypridle``` # idle management daemon
   10. ```hyprlang``` # library that implements parsing for the hypr configuration language
   11. ```hyprutils``` # library providing shared implementations of commonly used types
   12. ```hyprwayland-scanner``` # utility to generate sources and headers for wayland protocol
   13. ```hyprgraphics``` # library providing shared implementations of some utilities relating to graphics and resources
   14. ```grim``` # Screenshot utility for Wayland
   15. ```wf-recorder``` # Screen recorder
3. add ```exec-once = waybar``` into ~/.config/hypr/hyprland.conf
4. $ git clone https://aur.archlinux.org/yay.git
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
9.  $ yay -S 
   1. ```hyprlock``` # screen lock for Hyprland
   2. ```hyprpaper``` # wallpaper utility
   3. ```hyprpicker``` # utility for picking a color
   4. ```hyprsunset``` # blue light filter
   5. ```cliphist``` # Clipboard Manager [docs](https://github.com/sentriz/cliphist)
   6. only install the package that we have here
   7. if asked about dependency conflict press y to remove dependency
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
   

# End Goal 
* Setup a Nice Environment Like [end-4](https://github.com/end-4/dots-hyprland/tree/main) baked in with the taskbar like [HyDE](https://github.com/HyDE-Project/HyDE)
* https://wiki.hyprland.org/Getting-Started/Preconfigured-setups/

https://github.com/kurealnum/dotfiles/blob/main/.config/scripts/sysmaintenance.sh