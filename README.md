# tdarr-lxc-intel-arc
Documentation to use Intel ARC gpu in Proxmox with the tdarr lxc

# Install driver on Proxmox host
> [!IMPORTANT] 
First make sure that you at least have Proxmox 8.3!

```bash
apt install intel-media-va-driver vainfo
```
- Check if av1 support is enabled using the command `vainfo`

![Screenshot 2024-11-24 195431](https://github.com/user-attachments/assets/c233cbe2-d330-48f5-bfb1-64eaa89c07a2)


# Install tdarr LXC
- execute following command in the Proxmox terminal and follow the on-screen instructions
```bash
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/tdarr.sh)"
```

# Configure tdarr LXC

## Install HandBrake
```bash
apt remove handbrake-cli
apt install flatpak

wget https://github.com/HandBrake/HandBrake/releases/download/1.8.2/HandBrake-1.8.2-x86_64.flatpak
wget https://github.com/HandBrake/HandBrake/releases/download/1.8.2/HandBrake.Plugin.IntelMediaSDK-1.8.2-x86_64.flatpak

flatpak install HandBrake-1.8.2-x86_64.flatpak
flatpak install HandBrake.Plugin.IntelMediaSDK-1.8.2-x86_64.flatpak
```

## Wrapper
1. Create a new wrapper script
```bash
nano /opt/tdarr/handbroken
```
2. Add the following content to the file:
```bash
#!/usr/bin/bash
flatpak run --command=HandBrakeCLI fr.handbrake.ghb "$@"
```
3. Make the script executable
```bash
chmod +x /opt/tdarr/handbroken
```

## Tdarr configuration
1. Edit the `Tdarr_Node_Config.json` file:
``` bash
nano /opt/tdarr/configs/Tdarr_Node_Config.json
```
  - Press `Ctrl+X` `y` to exit.
2. Update the `handbrakePath` field to point to the wrapper:
``` bash
"handbrakePath": "/opt/tdarr/handbroken",
```
  - Press `Ctrl+X` `y` to exit.
3. Reboot tdarr
```bash
# This spawns a new extra node, not sure how to do this correctly
systemctl restart tdarr-node.service

# Reboot, the easy way
reboot now
```

- Check if handbrake is correctly installed using `HandBrakeCLI --version`

  # Tdarr config
  
- Add the plugin
    Tdarr_Plugin_075a_Transcode_Customisable
    Video Transcode Customisable
- Inputs
  
| Option              | Value
| ---                 | ---
| codecs_to_exclude   | av1
| cli                 | handbrake
| transcode_arguments | -Z "AV1 QSV 2160p 4K" --all-subtitles --all-audio
output_container | .mkv
