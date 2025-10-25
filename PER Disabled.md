---
tags: 
- PER
MOC: Personal
---
[[_0000 Home|Home]] | [[_0003 Personal MOC]]

# Disabled services
```Bash
# PostgreSQL database
sudo systemctl stop postgresql
sudo systemctl disable postgresql

# ClamAV antivirus (heavy scanner)
sudo systemctl stop clamav-freshclam
sudo systemctl disable clamav-freshclam

# VirtualBox services (if you're not using VBox)
sudo systemctl stop vboxdrv vboxautostart-service vboxballoonctrl-service vboxweb-service
sudo systemctl disable vboxdrv vboxautostart-service vboxballoonctrl-service vboxweb-service

# NFS server (if you don't share files over network)
sudo systemctl stop nfs-server nfs-blkmap rpcbind
sudo systemctl disable nfs-server nfs-blkmap rpcbind

systemctl --user stop tracker-miner-fs-3 
systemctl --user disable tracker-miner-fs-3

systemctl --user stop evolution-addressbook-factory evolution-calendar-factory evolution-source-registry 
systemctl --user disable evolution-addressbook-factory evolution-calendar-factory evolution-source-registry

sudo systemctl disable snapd.service 
sudo systemctl disable snapd.socket 
sudo systemctl disable snapd.seeded.service

systemctl --user disable xdg-desktop-portal-gnome 
systemctl --user disable xdg-desktop-portal-gtk

systemctl --user disable pipewire-media-session

systemctl --user disable evolution-addressbook-factory 
systemctl --user disable evolution-calendar-factory 
systemctl --user disable evolution-source-registry 
systemctl --user disable tracker-miner-fs-3

gsettings set org.freedesktop.Tracker3.Miner.Files crawling-interval -2
```