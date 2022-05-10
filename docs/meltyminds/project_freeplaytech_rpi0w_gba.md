# Freeplaytech RPi0W Game Boy

Page is dedicated to Game Boy on Raspberry Pi Zero W project made possible by [Freeplaytech](https://www.freeplaytech.com/) circuit board. 

- [Freeplay Zero DIY Kit (Old Model)](https://www.freeplaytech.com/product/freeplay-zero-diy-kit-old-model/)
- [Freeplay Setup](https://www.freeplaytech.com/support/setup/)

## Controller options

To [determine button values](https://retropie.org.uk/docs/RetroArch-Configuration/#determining-button-values) run following comand:
```
jstest /dev/input/js0
```
Replace js0 with js1, js2, js3, etc. as needed if not detected.

Auto configuration is stored at `/opt/retropie/configs/all/retroarch/autoconfig`.  
Remaps are saved as `.rmp` files in directory: `/opt/retropie/configs/SYSTEMNAME/`.

## Run ROMs form USB

1. Format USB.
2. Create folder `retropi-mount`
3. Plug into RPi. All necessary contents will be copied on USB drive. 

!!! info
    Once the folder structure is copied over the USB will be mounted over the RetroPie folder so any ROMs you add to your Pi will be run off of the USB.  
    [RetroPi Docs](https://retropie.org.uk/docs/Running-ROMs-from-a-USB-drive/)

## Case upgrade 

- [RetroModding - Backlight bleeding protector](https://www.thingiverse.com/thing:3723570)
- [RetroModding - Speaker color ring](https://www.thingiverse.com/thing:4168303)
- [RetroModding - Cart cover ventilated](https://www.thingiverse.com/thing:2810882)
- [RetroModding - Cart cover solid](https://www.thingiverse.com/thing:3689361)