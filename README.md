# tdarr-lxc-intel-arc
Documentation to use Intel ARC gpu in Proxmox with the tdarr lxc

# Install driver on Proxmox host
```bash
apt install intel-media-va-driver vainfo
```
- Check if av1 support is enabled using the command `vainfo`

# Install tdarr LXC
- execute following command in the Proxmox terminal
```bash
bash -c "$(wget -qLO - https://github.com/community-scripts/ProxmoxVE/raw/main/ct/tdarr.sh)"
```

# Configure tdarr LXC
- Run following commands to build handbrake:
```bash
apt remove handbrake-cli
apt install git
git clone https://github.com/HandBrake/HandBrake.git && cd HandBrake
./configure --launch-jobs=$(nproc) --launch --enable-qsv --disable-gtk
make --directory=build install
ln -s /usr/local/bin/HandBrakeCLI /usr/bin/HandBrakeCLI
```
- Check if handbrake is correctly installed using `HandBrakeCLI --version` in my case the version is 'HandBrake 20241124062553-9a59fb9cb-master'
