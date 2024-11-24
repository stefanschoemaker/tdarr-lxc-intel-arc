# tdarr-lxc-intel-arc
Documentation to use Intel ARC gpu in Proxmox with the tdarr lxc

# Install tdarr LXC
- execute following command in the Proxmox terminal
```bash
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/tdarr.sh)"
```

# Install driver on Proxmox host
```bash
apt install intel-media-va-driver vainfo
```
