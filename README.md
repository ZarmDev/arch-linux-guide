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

## 8. Install Base System
```bash
pacstrap /mnt base linux linux-firmware
```

## 8. Generate fstab
```bash
genfstab -U /mnt >> /mnt/etc/fstab
```

## 9. Chroot into the System
```bash
arch-chroot /mnt
```
Once you have "chrooted" into your installation, you are in the system that you will use after rebooting.

WARNING: You should install any neccessary packages now like NetworkManager to ensure you have access to wifi and dont have to reinstall Arch Linux (see next steps)
## 10. Configure System
```bash
ln -sf /usr/share/zoneinfo/Region/City /etc/localtime
hwclock --systohc
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen
echo "myhostname" > /etc/hostname
```

# 11. Install Bootloader (Choose One)

### Option A: `systemd-boot` (UEFI only and preferred if you are not dual booting)
```bash
bootctl install
```
Configure `/boot/loader/entries/arch.conf` manually with kernel and initrd.

### Option B: GRUB (preferred for dual booting)
```bash
pacman -S grub efibootmgr
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB
grub-mkconfig -o /boot/grub/grub.cfg
```

## 12. Create User
```bash
passwd                   # Set root password
useradd -m -G wheel <yourusername>
passwd <yourusername>
```

## 13. Enable Networking
### Option A: systemd-networkd
```bash
systemctl enable systemd-networkd
systemctl enable systemd-resolved
```

### Option B: NetworkManager
```bash
pacman -S networkmanager
systemctl enable NetworkManager
```

## 14. Exit and Reboot
```bash
exit
reboot
```

