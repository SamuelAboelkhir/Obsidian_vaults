---
tags: 
- services
- LI
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Linux MOC|Back to Linux MOC]] 

# WiFi Hotspot Setup with iwd & systemd

## Overview
Setting up a persistent WiFi access point on Arch Linux using iwd (instead of NetworkManager/hostapd) with automatic startup via systemd service.

## Prerequisites
- `iwd` installed and running
- `dnsmasq` for DHCP (alternative to iwd's built-in DHCP)
- `iptables` for NAT/forwarding
- Wireless interface (usually `wlan0`)

## Key Concepts

### Network Architecture
```
Internet (192.168.1.x) ←→ Laptop ←→ Hotspot (192.168.4.x)
                          ↑
                   Acts as bridge/router
```

**Why different IP ranges?**
- Prevents conflicts between existing network and hotspot
- Laptop acts as router between the two networks
- Devices connect to hotspot (192.168.4.x), laptop forwards to internet (192.168.1.x)

### Components Required
1. **Access Point** - iwd creates WiFi network
2. **DHCP Server** - Assigns IP addresses to connected devices  
3. **IP Forwarding** - Routes traffic between networks
4. **NAT/iptables** - Translates addresses for internet access

## Step-by-Step Implementation

### 1. Manual Hotspot Setup (Testing)

```bash
# Start access point
iwctl ap wlan0 start "HotspotName" "Password123"

# Assign IP to hotspot interface
sudo ip addr add 192.168.4.1/24 dev wlan0

# Start DHCP server (avoiding port 53 conflicts)
sudo dnsmasq --interface=wlan0 \
             --dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h \
             --dhcp-option=3,192.168.4.1 \
             --dhcp-option=6,8.8.8.8,8.8.4.4 \
             --port=0 \
             --pid-file=/var/run/dnsmasq-hotspot.pid

# Enable IP forwarding
echo 1 | sudo tee /proc/sys/net/ipv4/ip_forward

# Find internet interface
INET_INTERFACE=$(ip route | grep default | head -1 | awk '{print $5}')

# Set up NAT rules
sudo iptables -t nat -A POSTROUTING -s 192.168.4.0/24 -o "$INET_INTERFACE" -j MASQUERADE
sudo iptables -A FORWARD -i wlan0 -o "$INET_INTERFACE" -j ACCEPT
sudo iptables -A FORWARD -i "$INET_INTERFACE" -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT
```

### 2. Creating Persistent systemd Service

#### Service File: `/etc/systemd/system/wifi-hotspot.service`

```ini
[Unit]
Description=WiFi Hotspot using iwd
After=network.target iwd.service
Requires=iwd.service

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/usr/local/bin/hotspot-start.sh
ExecStop=/usr/local/bin/hotspot-stop.sh
TimeoutStartSec=60

[Install]
WantedBy=multi-user.target
```

#### Start Script: `/usr/local/bin/hotspot-start.sh`

```bash
#!/bin/bash

# Wait for system readiness
sleep 5

# Auto-detect internet interface
INET_INTERFACE=$(ip route | grep default | head -1 | awk '{print $5}')

# Start access point
iwctl ap wlan0 start "BlackDovah" "BLACK1234abcd"
sleep 3

# Configure hotspot IP
ip addr add 192.168.4.1/24 dev wlan0

# Start DHCP server
dnsmasq --interface=wlan0 \
         --dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h \
         --dhcp-option=3,192.168.4.1 \
         --dhcp-option=6,8.8.8.8,8.8.4.4 \
         --port=0 \
         --pid-file=/var/run/dnsmasq-hotspot.pid &

# Enable forwarding and NAT
echo 1 > /proc/sys/net/ipv4/ip_forward
iptables -t nat -A POSTROUTING -s 192.168.4.0/24 -o "$INET_INTERFACE" -j MASQUERADE
iptables -A FORWARD -i wlan0 -o "$INET_INTERFACE" -j ACCEPT
iptables -A FORWARD -i "$INET_INTERFACE" -o wlan0 -m state --state RELATED,ESTABLISHED -j ACCEPT

echo "Hotspot started, forwarding via $INET_INTERFACE"
```

#### Stop Script: `/usr/local/bin/hotspot-stop.sh`

```bash
#!/bin/bash

# Stop DHCP server
[ -f /var/run/dnsmasq-hotspot.pid ] && kill $(cat /var/run/dnsmasq-hotspot.pid)

# Stop access point
iwctl ap wlan0 stop

# Remove IP assignment
ip addr del 192.168.4.1/24 dev wlan0 2>/dev/null

echo "Hotspot stopped"
```

### 3. Service Management

```bash
# Make scripts executable
sudo chmod +x /usr/local/bin/hotspot-start.sh
sudo chmod +x /usr/local/bin/hotspot-stop.sh

# Enable service for auto-start
sudo systemctl enable wifi-hotspot.service

# Start service now
sudo systemctl start wifi-hotspot.service

# Check status
sudo systemctl status wifi-hotspot.service

# View logs
sudo journalctl -u wifi-hotspot.service
```

## Troubleshooting Guide

### Common Issues

**Interface disappears when stopping iwd:**
- iwd manages the wireless interface
- Stopping iwd removes `wlan0`
- Use iwd's built-in AP mode instead of NetworkManager

**dnsmasq port 53 conflicts:**
- Usually conflicts with `systemd-resolved`
- Solution: Use `--port=0` to disable DNS, only use DHCP
- Alternative: Configure systemd-resolved to not use port 53

**Devices connect but no internet:**
- Check IP forwarding: `cat /proc/sys/net/ipv4/ip_forward`
- Verify iptables rules: `sudo iptables -t nat -L -v -n`
- Confirm internet interface: `ip route | grep default`

**Hotspot doesn't survive reboot:**
- Manual configs are temporary
- iptables rules don't persist
- Solution: Use systemd service for persistence

### Debugging Commands

```bash
# Check wireless interface status
ip addr show wlan0
iw dev wlan0 info

# Monitor iwd logs
sudo journalctl -u iwd -f

# Check DHCP assignments
sudo journalctl -u wifi-hotspot.service

# Verify iptables rules
sudo iptables -L -v -n
sudo iptables -t nat -L -v -n

# Test connectivity from connected device
ping 192.168.4.1  # Should reach laptop
ping 8.8.8.8      # Should reach internet
```

## systemd Service Creation Guide

### General Service Structure

```ini
[Unit]
Description=Brief description of service
After=dependency.service
Requires=hard-dependency.service
Wants=soft-dependency.service

[Service]
Type=oneshot|simple|forking
ExecStart=/path/to/start-script
ExecStop=/path/to/stop-script
RemainAfterExit=yes  # For oneshot services
User=username        # Optional: run as specific user

[Install]
WantedBy=multi-user.target  # Start at boot
```

### Service Types
- **`oneshot`**: Runs once, exits (good for setup scripts)
- **`simple`**: Long-running process, doesn't fork
- **`forking`**: Process creates child and parent exits

### Key systemd Commands

```bash
# Reload systemd after creating/editing services
sudo systemctl daemon-reload

# Enable service for auto-start
sudo systemctl enable service-name.service

# Start/stop/restart service
sudo systemctl start service-name.service
sudo systemctl stop service-name.service  
sudo systemctl restart service-name.service

# Check service status
sudo systemctl status service-name.service

# View service logs
sudo journalctl -u service-name.service
sudo journalctl -u service-name.service -f  # Follow logs
```

## Key Learnings

### Network Management Conflicts
- **iwd vs NetworkManager**: Can't run both simultaneously
- **GUI WiFi managers**: Often reset manual configurations
- **Solution**: Choose one network manager and stick with it

### Persistence Challenges
- Manual `ip`, `iptables` commands don't survive reboots
- systemd services provide clean startup/shutdown automation
- Scripts should handle errors gracefully (`2>/dev/null`)

### DHCP Considerations
- iwd has built-in DHCP but may not work reliably
- dnsmasq is more robust but needs conflict resolution
- Port conflicts are common (DNS port 53)