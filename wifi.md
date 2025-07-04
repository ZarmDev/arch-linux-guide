1. Install NetworkManager
sudo pacman -S networkmanager

2. Enable the service
sudo systemctl enable NetworkManager
sudo systemctl start NetworkManager

3. Use the CLI (or optionally the GUI with nmtui)
nmcli device wifi list
nmcli device wifi connect "SSID" password "your_password"

4. Check if connected
nmcli device status
