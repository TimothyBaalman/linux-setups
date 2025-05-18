# Hyprland with Nvidia
1. pacman -S nvidia nvidia-utils egl-wayland lib32-nvidia-utils xorg --ignore xorg-server-xdmx sddm sddm-runit hyprland git kitty packagekit-qt5 
2. accept defaults with enter unless you want something else
3. $ ln -s /etc/runit/sv/sddm /run/runit/service
   1. Your sddm will start automatically after this
   2. to get into tty press alt+(one of the function keys mine was F4)F1-6

# Hyprland Installs
1. $ git clone https://aur.archlinux.org/yay.git
   1. $ cd yay
   2. $ makepkg -si
2. $ sudo pacman -S mako pipewire wireplumber xdg-desktop-portal-hyprland qt5-wayland qt6-wayland waybar wofi wayland-protocols dolphin hyprcursor hypridle hyprlang hyprutils hyprwayland-scanner hyprgraphics
3. add ```exec-once = waybar``` into ~/.config/hypr/hyprland.conf
4. $ sudo reboot
5. $ yay -S hyprlock hyprpaper hyprpicker hyprsunset
   1. only install the package that we have here
   2. if asked about dependency conflict press y to remove dependency


https://github.com/kurealnum/dotfiles/blob/main/.config/scripts/sysmaintenance.sh