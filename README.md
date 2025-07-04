# Simple Arch installation guide (WIP)
## 1. This generally, works for any linux distributions, install the arch ISO and then flash it to the USB using Rufus or BalencaEtcha.
   - Plug USB
   - Under select, select the ISO file (Rufus)
   - Ensure partition scheme matches your system (UEFI is most people's option which is for modern systems. MBR is legacy BIOS)
   - Leave everything else as default
   - If it prompts "Please select the mode that you want to use to write this image." then you can go with ISO image mode, but if it does not work then use the other mode next time.
## 2. (Optional) Verify the checksum
   - Go to "Checksums and signatures" and go to ISO and then download the PGP signature.
   - Install gpg from https://gnupg.org/
   - Then, run gpg --keyserver-options auto-key-retrieve --verify archlinux-version-x86_64.iso.sig
## 3. (Optional) Dualing booting
   - You can partition your drive (which just means creating space for other operating systems or other files)
   - You can use Disk Management on windows or on both Linux, Windows and Mac there are command line options.
   - You also may need to delete a partition like a recovery partition in order to join volumes together:

   1.Open Command Prompt as Administrator
   
   2.Press Windows + X → select Command Prompt (Admin) or Windows Terminal (Admin).
      
   3.Launch DiskPart using ```diskpart```
   
   4. ```list disk```
   
   5. select disk X   ← Replace X with the disk number
   
   6.list partition
   
   7.Find the recovery partition - it’s usually small (500MB–1GB) and has a label of “Recovery.”
   
   8.select partition Y   ← Replace Y with the recovery partition number
   
   9.delete partition override ← The override flag is necessary because recovery partitions are protected.

## 4. Plug it into your PC
   - Enter BIOS/UEFI (usually by pressing F2, F12, ESC, or DEL during boot)
   - Select the USB drive as the boot device
## 5. Connect to WIFI (note you will be unable to do this when you leave the ISO unless you install it after rebooting/chroot steps)
   - In your ISO, you will be able to use the command ```iwctl```
   
1.First, use device list to find your device: ```device list```

2.A device is the wifi adapter that you want to use. Usually, if it's wireless you can just use wlan0

3.If you already know your device (WIFI network name) then just do ```station <device> connect <SSID/wifinetworkname>```

4.Then, put the password.

5.If not, first run ```station device_name scan``` and then ```station device_name get-networks``` and then connect to the wifi based on it's name.

## 6. 
