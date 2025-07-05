## Intel
> To acquire updated microcode, depending on the processor, install one of the following packages:
>
> amd-ucode for AMD processors,
>
> intel-ucode for Intel processors.

sudo pacman -S <thepackagebasedonabove>

# Only if your using grub
sudo grub-mkconfig -o /boot/grub/grub.cfg

# Only if your using systemd-boot
```
sudo nano /boot/loader/entries/arch.conf
# Then, add this BEFORE initrd /initramfs-linux.img
initrd /intel-ucode.img
```
