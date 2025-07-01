## Refresh package database
sudo pacman -Syyu
## Setting up reflector
Reflector is a tool that finds sites to use for downloading packages and other tools
```
sudo pacman -S reflector
// Grabs the 10 most recently updated mirrors and sorts them by speed
sudo reflector --latest 10 --sort rate --save /etc/pacman.d/mirrorlist
```
