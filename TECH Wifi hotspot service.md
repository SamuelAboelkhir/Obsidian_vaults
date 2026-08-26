---
tags: 
- services
- LI
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Technology MOC|Back to Technology MOC]] | [[TECH Linux index|Back to index]]
### Service
`/etc/systemd/system/wifi-hotspot.service`
```
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
### Start and stop scripts
`/usr/local/bin/hotspot-start.sh`
```bash
#!/usr/bin/env bash
set -Eeuo pipefail

# Wait for system readiness
sleep 5

# Auto-detect internet interface
INET_INTERFACE=$(ip route get 1.1.1.1 | awk '{print $5; exit}')
echo "$INET_INTERFACE" >/run/hotspot-iface

# Start access point
iwctl ap wlan0 start "BlackDovah" "BLACK1234abcd"
sleep 3

# Configure hotspot IP
ip addr replace 192.168.4.1/24 dev wlan0

# Start DHCP server
dnsmasq --interface=wlan0 \
	 --bind-dynamic \
         --dhcp-range=192.168.4.2,192.168.4.20,255.255.255.0,24h \
         --dhcp-option=3,192.168.4.1 \
         --dhcp-option=6,8.8.8.8,8.8.4.4 \
         --port=0 \
         --pid-file=/run/dnsmasq-hotspot.pid &

# Enable forwarding and NAT
sysctl -w net.ipv4.ip_forward=1
iptables -t nat -C POSTROUTING \
    -s 192.168.4.0/24 \
    -o "$INET_INTERFACE" \
    -j MASQUERADE 2>/dev/null ||
iptables -t nat -A POSTROUTING \
    -s 192.168.4.0/24 \
    -o "$INET_INTERFACE" \
    -j MASQUERADE
iptables -C FORWARD \
    -i wlan0 \
    -o "$INET_INTERFACE" \
    -j ACCEPT 2>/dev/null || \
iptables -A FORWARD \
    -i wlan0 \
    -o "$INET_INTERFACE" \
    -j ACCEPT

iptables -C FORWARD \
    -i "$INET_INTERFACE" \
    -o wlan0 \
    -m conntrack --ctstate RELATED,ESTABLISHED \
    -j ACCEPT 2>/dev/null || \
iptables -A FORWARD \
    -i "$INET_INTERFACE" \
    -o wlan0 \
    -m conntrack --ctstate RELATED,ESTABLISHED \
    -j ACCEPT

echo "Hotspot started, forwarding via $INET_INTERFACE"
```
`/usr/local/bin/hotspot-stop.sh`
```bash
#!/usr/bin/env bash
set -Eeuo pipefail

# Stop DHCP server
[ -f /run/dnsmasq-hotspot.pid ] && kill $(cat /run/dnsmasq-hotspot.pid)

# Stop access point
iwctl ap wlan0 stop

# Remove IP assignment
ip addr del 192.168.4.1/24 dev wlan0 2>/dev/null

INET_INTERFACE=$(cat /run/hotspot-iface)
rm -f /run/hotspot-iface

iptables -t nat -D POSTROUTING \
    -s 192.168.4.0/24 \
    -o "$INET_INTERFACE" \
    -j MASQUERADE 2>/dev/null || true

iptables -D FORWARD \
    -i wlan0 \
    -o "$INET_INTERFACE" \
    -j ACCEPT 2>/dev/null || true

iptables -D FORWARD \
    -i "$INET_INTERFACE" \
    -o wlan0 \
    -m conntrack \
    --ctstate RELATED,ESTABLISHED \
    -j ACCEPT 2>/dev/null || true

echo "Hotspot stopped"
```
