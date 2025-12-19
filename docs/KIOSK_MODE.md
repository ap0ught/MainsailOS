# Kiosk Mode for Touch Displays

MainsailOS includes built-in support for displaying the Mainsail web interface in kiosk mode on touch displays. This feature allows you to connect a display directly to your Raspberry Pi or SBC and have the Mainsail interface automatically displayed on boot.

## How It Works

When MainsailOS detects a connected display (framebuffer device or DRM card), it will automatically:

1. Autologin the `pi` user on tty1
2. Start an X server session
3. Launch Chromium browser in kiosk mode
4. Display the Mainsail interface at `http://localhost`

The kiosk mode includes the following optimizations:

- Screen blanking and power management are disabled
- Mouse cursor automatically hides when idle
- Chromium runs in full-screen kiosk mode without UI elements
- No error dialogs or translation prompts

## Automatic Detection

Kiosk mode will automatically start if:

- A framebuffer device is present (`/dev/fb0`), OR
- A DRM graphics card is detected (`/sys/class/drm/card0`)

This means:
- **With display attached**: Kiosk mode starts automatically
- **Headless setup**: Normal console login is preserved

## Disabling Kiosk Mode

If you have a display connected but prefer to use the console instead of kiosk mode, you can disable automatic startup:

```bash
sudo disable-kiosk
sudo reboot
```

After disabling, you can still manually start the kiosk interface by running:

```bash
startx
```

## Manual Configuration

If you want to customize the kiosk mode behavior, you can edit the following files:

### Chromium Configuration

Edit `/home/pi/.xinitrc` to customize Chromium startup options:

```bash
nano /home/pi/.xinitrc
```

For example, you can:
- Change the URL (default is `http://localhost`)
- Add or remove Chromium flags
- Adjust the startup delay

Note: The script automatically detects whether to use `chromium` or `chromium-browser` depending on your system.

### Autologin Configuration

The autologin configuration is stored in:
```
/etc/systemd/system/getty@tty1.service.d/autologin.conf
```

## Troubleshooting

### Black Screen on Boot

If you see a black screen:

1. Check if Mainsail is accessible from another device at `http://<your-pi-ip>`
2. Try SSHing in and checking X server logs: `cat /home/pi/.local/share/xorg/Xorg.0.log`
3. Verify Chromium is running: `ps aux | grep chromium`

### Browser Shows Error Page

If Chromium displays an error:

1. Wait 30-60 seconds for services to start
2. Check if Moonraker is running: `sudo systemctl status moonraker`
3. Check if nginx is running: `sudo systemctl status nginx`

### Want to Use SSH with Display Connected

You can still SSH into your system even with kiosk mode active. The kiosk mode only affects tty1 (the physical display).

## Hardware Requirements

- **Display**: Any HDMI/DSI display compatible with your SBC
- **Touch**: Optional - any display with touch support will work
- **Resources**: Approximately 200-300MB additional RAM for X server and Chromium

## Performance Notes

- Kiosk mode uses additional system resources
- For best performance, use at least a Raspberry Pi 3 or equivalent
- On lower-end boards, there may be a slight delay when loading the interface

## Related Resources

- [Security Considerations](KIOSK_MODE_SECURITY.md) - Important security information about kiosk mode
- Original inspiration: [Display Mainsail instead of KlipperScreen](https://github.com/Contrasens/VCore3-400-OpenFront/blob/main/Display_Mainsail_instead_of_KlipperScreen.md)
- [Mainsail Documentation](https://docs.mainsail.xyz)
- [MainsailOS Documentation](https://docs-os.mainsail.xyz)
