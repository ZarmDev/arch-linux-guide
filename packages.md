# Starting with packages
## Setting up reflector
Reflector is a tool that finds sites to use for downloading packages and other tools
```
sudo pacman -S reflector
# Needed by reflector
sudo pacman -S rsync
```
Now run `sudo nano /etc/xdg/reflector/reflector.conf` and add these lines without or with the comments:
```
# Set the output path where the mirrorlist will be saved (--save).
--save /etc/pacman.d/mirrorlist

# Select the transfer protocol (--protocol).
--protocol https

# Select the country (--country).
# Consult the list of available countries with "reflector --list-countries>
# select the countries nearest to you or the ones that you trust. For exam>
--country "United State"

# Use only the  most recently synchronized mirrors (--latest).
--latest 5

# Sort the mirrors by synchronization time (--sort).
--sort age
```
Then enable the service:
```
systemctl start reflector.service
```
# Troubleshooting 404 errors or other mirror errors
## Refresh package database
sudo pacman -Syyu
## Clear package cache
sudo pacman -Scc
