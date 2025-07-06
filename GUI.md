# GUI
You have several options for your GUI when setting up Arch Linux. Here are my personal suggestions:

## Before you chose a desktop environment (DE)...
Note that most DEs either use X11 or Wayland as their display manager.

### X11 vs Wayland
**TLDR: Wayland is the modern option that is better for most use cases.**
- Ease of use: Wayland easily wins this one (read complexity below)
- Compatability: A older display manager that has good compatibility
- Performance: This depends on your setup but X11 probably takes less battery/RAM but is less smooth in terms of rendering
- Complexity: X11 relies on third party "compositors" that:
  1. Add window shadows, transparency, blur, and animations
  2. Help reduce screen tearing by managing frame buffers
  3. Enable smooth transitions and desktop effects
  
  On the other hand, Wayland handles this by itself.
- App isolation/Security: In X11, any app can see what other apps are doing or inject fake input.


## If you have a laptop/touchscreen OR you want a nice and clean interface, use GNOME
It provides a touchscreen keyboard and out of the box support for volume, brightness and more. It's very easy to setup and it has a great interface.

## If you are looking for the best performance and want the "Arch experience" then use i3wm OR Sway
NOTE: This is a "tiling window manager" meaning it will be nothing like the Windows GUI, instead you will rely on keyboard shortcuts most of the time.

**Before you decide on i3wm or Sway, read the note above about X11 vs Wayland. Or if you don't care use Wayland**
## If you are looking for a desktop environment similar to Windows (simple), then use KDE plasma or Cinnamon
They have familar interfaces for those who have used Windows. 

A lot of people seem to like KDE plasma because their dev team has piloted many things like the app, KDE Connect, which lets you see text messages, files, etc on your phone

(and lets just say KDE has a lot more sponsors...)
