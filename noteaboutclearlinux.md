# Make your device like a tablet
## This may make your physical battery worse and take more energy. But, if you use it very often then it's better than turning it on and off
## Make sure optimizations/system knows you want a "desktop"
clr_power --desktop
Options are:

-d, --debug Display debug/verbose output including built-in values.

-S, --server Treat the system as a server, and apply server specific tunings.

-D, --desktop Treat the system as a desktop, and apply desktop specific tunings

## Prevent hibernation/sleep and optionally suspend
sudo systemctl mask sleep.target hibernate.target hybrid-sleep.target suspend.target

## Ignore lid closing
sudo nano /etc/systemd/logind.conf

Then, change HandleLidSwitch to ignore:

HandleLidSwitch=ignore

Restart PC or restart service

sudo systemctl restart systemd-logind

