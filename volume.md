# Easily change volume (GUI)
```
sudo pacman -S pipewire pavucontrol
```
Then, check if it's running, if this returns nothing then its running
```
pulseaudio --check
```
Then, run this to get a GUI for controling volume
```
pavucontrol
```
If it does not work try to restart the service:
```
systemctl --user restart pulseaudio.service
```
