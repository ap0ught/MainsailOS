# Security Considerations for Kiosk Mode

## Overview

The kiosk mode implementation has been designed with security in mind while balancing usability for embedded 3D printer systems.

## Security Features

### 1. Localhost-Only Connection
- The browser only connects to `http://localhost`
- No external network connections are initiated by the kiosk
- All printer control remains on the local system

### 2. Autologin Limited to Physical Console
- Autologin is configured only for tty1 (physical display)
- SSH access still requires authentication
- Serial console access still requires authentication
- Only affects users with physical access to the device

### 3. No Credential Exposure
- No passwords or API keys are hardcoded in the scripts
- All authentication is handled by the existing Mainsail/Moonraker setup
- Browser session security follows standard Chromium practices

### 4. File Permissions
- Configuration files have appropriate ownership (pi:pi)
- Scripts are executable only where needed
- No world-writable files created

### 5. User Isolation
- Kiosk runs as the regular `pi` user, not root
- Standard Linux user permissions apply
- No privilege escalation vectors introduced

## Potential Security Considerations

### Physical Access
- Anyone with physical access to the display can interact with the printer
- This is by design for a kiosk system
- Users who need more security should:
  - Not enable kiosk mode
  - Configure Mainsail authentication
  - Keep the device in a secure location

### Network Security
- Kiosk mode does not change network security
- Mainsail is still accessible over the network as configured
- Users should follow standard network security practices:
  - Use strong WiFi passwords
  - Consider VPN for remote access
  - Enable Moonraker authentication if needed

### Browser Security
- Chromium runs with standard security features enabled
- No `--no-sandbox` or similar security-reducing flags
- Browser updates follow system package updates

## Disabling Kiosk Mode

Users can disable kiosk mode at any time by:
1. SSH into the system
2. Run `sudo disable-kiosk`
3. Reboot

This restores standard console login requiring password authentication.

## Recommendations

For users concerned about physical security:
1. Use the `disable-kiosk` command
2. Set a strong password for the `pi` user
3. Consider disabling autologin entirely
4. Enable Moonraker authentication for API access
5. Configure firewall rules to limit network access

## Compliance

This implementation:
- Does not store or transmit sensitive data
- Uses only localhost connections for the browser
- Follows standard Linux security practices
- Maintains existing security boundaries
