# Hyprland with Nvidia
1. pacman -S nvidia nvidia-utils egl-wayland lib32-nvidia-utils xorg --ignore xorg-server-xdmx sddm sddm-runit hyprland git kitty packagekit-qt5 
2. accept defaults with enter unless you want something else
3. $ ln -s /etc/runit/sv/sddm /run/runit/service
   1. Your sddm will start automatically after this
   2. to get into tty press alt+(one of the function keys mine was F4)F1-6
4. 