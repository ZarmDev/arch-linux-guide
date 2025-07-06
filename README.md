# Simple Arch installation guide (if you are not dual booting)
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
   - Use right/left arrow keys to go to "Boot"
   - Select the USB drive and move it to the top by using the options on the right (probably using F6 button or +/-)
## 5. Connect to WIFI (note you will be unable to do this when you leave the ISO unless you install it after rebooting/chroot steps)
   - In your ISO, you will be able to use the command ```iwctl```
   
1.First, use device list to find your device: ```device list```

2.A device is the wifi adapter that you want to use. Usually, if it's wireless you can just use wlan0

3.If you already know your device (WIFI network name) then just do ```station <device> connect <SSID/wifinetworkname>```

4.Then, put the password.

5.If not, first run ```station device_name scan``` and then ```station device_name get-networks``` and then connect to the wifi based on it's name.

## 6. Format the Disk (NOT for dual booting, destroys all old files)
Essentially, when you try to install arch linux, your formatted usb should have an "EFI System Partition" which is where boot files are stored and the root partition which is where you want to put arch linux.
  - The **EFI System Partition** usually is a size of `512MB`, a file system of `FAT32`, and called `EFI`
  - The **Root Partition** is usually a big partition (that you should know if you partitioned earlier) using a file system of `ext4` or `btrfs`

Most of the time, EFI is partition 1 and root partition is partition 2. You can see this using ```lsblk```

We also have to format the two partitions because the arch operating system needs the filesystem to be structured in a way that it expects. For example, windows uses NTFS, but Arch Linux on the other hand uses ext4.

Technically, you wouldn't neccesarily have to to run these steps if you already had arch linux installed before and wanted to preserve the files. If you had any other OS prior, you would want it to be formatted to remove old remnants of that OS.
```bash
mkfs.fat -F32 /dev/sda1         # EFI
mkfs.ext4 /dev/sda2             # Root
```
If it says it contains a ext4 file system labelled "root" just proceed anyway by typing "y"

## 7. Mount Partitions
```bash
mount /dev/sda2 /mnt
mkdir /mnt/boot
mount /dev/sda1 /mnt/boot
```

Why do we mount?

Basically, we already have the two partitions, sda1 and sda2 existing on the physical disk. However, we want to actually have the linux kernel recognize that raw data (which is in binary) as files. So, when you mount, it checks the type of filesystem (for sda1, FAT32 and for sda2, ext4), loads drivers to handle it and then validates the "superblocks" and "inode structure" for integrity (if it exists without any errors or corruption)

If you didn't mount, the files for the core of Arch Linux would not exist after rebooting because you did not give it somewhere to place its files. Normally, as the command shows, sda2 would be mounted (put in) /mnt and /mnt/boot would contain sda1 but those directories would not exist and if you continued by using a bootloader, the bootloader would fail to find the boot files (causing crashes, errors)

## 8. Install Base System
```bash
pacstrap /mnt base linux linux-firmware
```

## 8. Generate fstab
As the Arch Installation Guide mentions: [Configure the system](https://wiki.archlinux.org/title/Installation_guide#Pre-installation)
> "To get needed file systems (like the one used for the boot directory /boot) mounted on startup, generate an fstab file. Use -U or -L to define by UUID or labels, respectively:
"
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

## 9. Chroot into the System
```bash
arch-chroot /mnt
```
Once you have "chrooted" into your installation, you are in the system that you will use after rebooting.

WARNING: You should install any neccessary packages now like NetworkManager to ensure you have access to wifi and dont have to reinstall Arch Linux (see next steps)
## 10. Configure Time
1. Finding the timezone
```bash
# List timezones 
timedatectl list-timezones
# If you can't scroll through the timezones, just run it again with grep (a way to filter for a word)
# Here's an example if you live in Tokyo
timedatectl list-timezones | grep Tokyo
```
2. Creating a symbolic link
A symbolic link is like a shortcut in windows. When you double click the app in your desktop folder, it doesn't actually exist there but usually instead points to an exe located somewhere else. In Linux, you can use symbolic links to reference a file without copying it over (but it will break if the original file is deleted)

Arch Linux already creates the timezones in /usr/share/zoneinfo and you just are adding a shortcut to one in /etc/localtime. /etc contains many configuration files like your langauge, user accounts, etc. According to copilot, these are some of the files you could find in /etc:
```
/etc/passwd & /etc/group Define user accounts and group memberships. These are foundational for login and permission management.

/etc/hostname Stores the system’s name — what it calls itself on a network.

/etc/fstab Contains mount instructions for partitions and devices. This is what tells your system how to mount /dev/sda2 as /, for example.

/etc/hosts Maps IP addresses to hostnames locally. Useful for overriding DNS or defining internal names.

/etc/pacman.conf Configuration for Arch’s package manager, pacman. Controls repositories, options, and hooks.

/etc/locale.conf Sets the system’s language and regional formatting.

/etc/sudoers Defines who can use sudo and under what conditions. Edited with visudo to avoid syntax errors.

/etc/systemd/ Contains service unit files and systemd configuration. This is where you manage what starts at boot.

/etc/X11/, /etc/ssh/, /etc/network/ Subdirectories for specific services and daemons — like graphical display, secure shell access, and networking.
```
You should note that Arch Linux uses the so-called KISS approach (Keep It Simple, Stupid):

Most of the files in /etc are:

1. Plain text — easy to read and edit

2. Human-readable — no binary/raw data

3. Modular — changes in one file don’t cause a domino effect of changes in other files

```bash
# Now you want to create a symbolic link
ln -sf /usr/share/zoneinfo/Country/City /etc/localtime
# Then, you want to synchronize your physical hardware clock (make sure your physical clock follows the time zone you want to use)
hwclock --systohc
```
## 11. Configure locale (keyboard language)
```
# Write this string into the file /etc/locale.gen
# If you don't want to use US, then you have to find the locales online since they aren't automatically generated. For example, British would be en_GB.UTF-8 UTF-8
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
# Generate locale
locale-gen
```

## 12. Hostname
Hostname is what your computer is called and this will be important when using SSH, for some apps and when doing DNS related things. Call it whatever you want!
```
echo "myhostname" > /etc/hostname
```

# 13. Install Bootloader (Choose One)

### Option A: `systemd-boot` (UEFI only and preferred if you are not dual booting)
```bash
bootctl install
```
Now configure `/boot/loader/entries/arch.conf`
```bash
# Install nano
pacman -S nano
# Check if you have the img files (important boot files)
# It should have files like: EFI, initramfs-linux-fallback.img, initramfs-linux.img, loader, vmlinuz-linux
ls /boot
```
You will edit a file called arch.conf and here is an explanation of what you will need to add:
1. title
   - whatever you want to show up when you boot
   - it will let you enter either the fallback or your actual main arch installation
2. linux
   - Your linux image
3. initrd
   - Loads essential kernel modules (e.g. for disk controllers, filesystems, encryption, LVM) **before** your actual OS loads
   - Detects and mounts the real root filesystem (like /dev/sda2)
   - Then, gives control to the real system via pivot_root or switch_root
4. options
   - The bootloader needs to know which part of the disk you want to load. Remember that those who dual boot will probably have sda1, sda2 and an extra sda3 partition. You need to tell the bootloader which partition you want to load.
   - You need to give it a PARTUUID which is a unique identification for that partition
You have to jot down the PARTUUID of your arch installation, which, assuming your not dual booting, should be sda2's PARTUUID.

Run the command and find the PARTUUID of /dev/sda2 and write it down or take a photo:
```bash
blkid
```
Then, use the text editor, nano, to edit the config file
```bash
# After this, a text editor will load and you can follow the instructions below
nano /boot/loader/entries/arch.conf
```
The actual file should look like what is shown below, the space between the first part and the second part can be one space or even a tab.

WARNING: ENSURE THAT AFTER THE PARTUUID YOU HAVE "rw"

Without it, it makes your **disk read-only instead of read and write** which means that services running on startup may be unable to write to the disk and it could cause some problems.

Add this to the file:
```
title   Arch Linux
linux   /vmlinuz-linux
initrd  /initramfs-linux.img
options root=PARTUUID=put-your-partuuid-here rw
```

Then, press CRTL+X, type y and press enter to write the contents of the file.

### Option B: GRUB (preferred for dual booting) (TODO FIX THIS)
```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

## 14. Create User
```bash
# Set the root password. This is for the root which can be accessed by running "su -" but it's recommended to use sudo instead (see later chapter)
passwd
# Add your user
useradd -m -G wheel <yourusername>
# Create a password specifically for your user
passwd <yourusername>
```

## 15. Enable Networking
### Option A: NetworkManager (PREFERRED)
```bash
pacman -S networkmanager
systemctl enable NetworkManager
```

### Option B: systemd-networkd
```bash
systemctl enable systemd-networkd
systemctl enable systemd-resolved
```

NOTE: Please do not install both or multiple network managers because it could later interfere with your VPN or in other cases

## 16. (Optional) Install sudo
You can do this anytime, but it's good practice to use sudo. It let's you run commands that require root just by putting "sudo" before the command
```bash
pacman -S sudo
# Add your user to have "sudo permissions"
usermod -aG wheel yourusername
# Edit config
nano /etc/sudoers
# Uncomment this line (remove #)
%wheel ALL=(ALL:ALL) ALL
```
Then CRTL+X, press y and then enter.

## 16. Exit and Reboot
```bash
exit
reboot
```
If you see the arch install option again like when you first plugged in your USB, that means you need to go to BIOS (or just use arrow keys to go to "Reboot into Firmware Interface") and then you have to make sure your previous option which is either called HDD or Arch or something like that is the first option.

If you successfully boot into arch linux (if it doesn't show you the ISO screen with "arch install medium" or something unexpected) then you now have a working arch installation! Congrats!!!! 🥳

**NOTE: You will have to connect to WIFI again when you reboot your computer. See wifi.md for more information.**

## 17. Next steps
1. Read wifi.md to ensure you have working wifi when you turn on your PC
2. Read packages.md to ensure you have fast mirrors to install your packages
3. Read microcode.md to ensure that your system is patched for CPU vulnerabilities and to prevent firmware issues
4. Read GUI.md for how to setup a GUI
5. Read improvingbattery.md to optimize your installation for better battery life
