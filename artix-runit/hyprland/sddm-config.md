# SDDM Config
1. sudo pacman -S --needed qt5‑graphicaleffects qt5‑quickcontrols2 qt5‑svg
2. Enable NumLock
3. $ sudo vim /etc/sddm.conf
   ```vim
      [General]
      Numlock=on
   ```
4. Choosing a theme
   1. [AUR for sddm themes](https://aur.archlinux.org/packages?O=0&SeB=nd&K=sddm+theme&outdated=&SB=p&SO=d&PP=50&submit=Go)
   2. I like sddm-theme-sugar-candy and sddm-theme-mountain-git
   3. $ yay -Sy sddm-theme-sugar-candy sddm-theme-mountain-git
   4. Enabling sugar-candy add to /etc/sddm.conf
   ```vim
   [Theme]
   Current=Sugar‑Candy
   ```

5. edit the /usr/share/sddm/themes/Sugar-Candy/theme.conf file for customizing config. I changed from the default
   1. FormPosition="center"
   2. PartialBlur="false"
   3. HourFormat="HH:mm:ss"
   4. DateFormat="dddd, MMMM d yyyy" [date format](https://doc.qt.io/archives/qt-5.15/qml-qtqml-date.html)
6. Test with $ sddm-greeter --test-mode --theme /usr/share/sddm/themes/sugar-candy