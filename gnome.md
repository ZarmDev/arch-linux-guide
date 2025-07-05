## Gnome Desktop Environment
```sudo pacman -S gnome gdm```

In the resulting options, choose whatever you want.

```
sudo systemctl enable gdm
sudo systemctl start gdm
```

## Battery life
## 1. Extensions
Either use these commands:
```
gnome-extensions list
gnome-extensions disable extension-name@domain
```
Or preferably, open the extensions app and disable all extensions
## 2. Use TLP
```
sudo pacman -S tlp
sudo systemctl enable tlp 
```
After starting servie it will automatically:

- Reduces CPU frequency when idle

- Enables disk and USB autosuspend (?)

- Disables unused radio devices (like Bluetooth)

- Tweaks PCIe and SATA power management (?)

- Activates audio power-saving features (?)
## 3. Other tips
1. Go to search (in the settings app) and then disable App Search
2. Disable lock screen notifications
3. Disable multitasking
4. In the "Software" app, turn off automatic updates
