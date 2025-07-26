# Installing XFCE
## Step 1
(xfce4-goodies is optional and contains useful plugins)

```sudo pacman -S xfce4 xfce4-goodies```
## Step 2 - Install a display manager
This is an example using lightdm, but there are other options.
```
sudo pacman -S lightdm lightdm-gtk-greeter
```
**In case of errors, uninstall your current desktop environment**

For example with GNOME:
```
sudo systemctl disable gdm.service
sudo pacman -Rns gnome gnome-shell gdm
sudo ln -sf /usr/lib/systemd/system/lightdm.service /etc/systemd/system/display-manager.service
```
Then, enable lightdm
```
sudo systemctl enable lightdm
```

## Step 3 - Reboot
```
sudo reboot
```
