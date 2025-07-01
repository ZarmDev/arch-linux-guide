# Starting with packages
## Setting up reflector
Reflector is a tool that finds sites to use for downloading packages and other tools
```
sudo pacman -S reflector
// Grabs the 10 most recently updated mirrors and sorts them by speed
sudo reflector --latest 10 --sort rate --save /etc/pacman.d/mirrorlist
```
# Troubleshooting 404 errors or other mirror errors
## Refresh package database
sudo pacman -Syyu
## Clear package cache
sudo pacman -Scc
