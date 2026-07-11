---
tags: 
- CLI
- LI
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Technology MOC|Back to Technology MOC]] | [[TECH Linux index|Back to index]]
### Service
`/etc/systemd/system/vm-firewall-rules.service`
```
[Unit]
Description=Libvirt forwarding fix
After=libvirtd.service
Requires=libvirtd.service

[Service]
Type=oneshot
RemainAfterExit=yes

ExecStart=/usr/local/bin/libvirt-network-fix.sh
ExecStop=/usr/local/bin/libvirt-network-fix-stop.sh

[Install]
WantedBy=multi-user.target
```
### Start and stop scripts
`/usr/local/bin/libvirt-network-fix.sh`
```bash
#!/usr/bin/env bash
set -Eeuo pipefail

EXT_IFACE=$(ip route get 1.1.1.1 | awk '{print $5; exit}')

iptables -C FORWARD \
    -i virbr0 \
    -o "$EXT_IFACE" \
    -j ACCEPT 2>/dev/null ||
iptables -I FORWARD 1 \
    -i virbr0 \
    -o "$EXT_IFACE" \
    -j ACCEPT

iptables -C FORWARD \
    -i "$EXT_IFACE" \
    -o virbr0 \
    -m conntrack --ctstate RELATED,ESTABLISHED \
    -j ACCEPT 2>/dev/null ||
iptables -I FORWARD 1 \
    -i "$EXT_IFACE" \
    -o virbr0 \
    -m conntrack --ctstate RELATED,ESTABLISHED \
    -j ACCEPT
```
`/usr/local/bin/libvirt-network-fix-stop.sh`
```bash
#!/usr/bin/env bash
set -Eeuo pipefail

EXT_IFACE=$(ip route get 1.1.1.1 | awk '{print $5; exit}')

iptables -D FORWARD \
    -i virbr0 \
    -o "$EXT_IFACE" \
    -j ACCEPT 2>/dev/null || true

iptables -D FORWARD \
    -i "$EXT_IFACE" \
    -o virbr0 \
    -m conntrack --ctstate RELATED,ESTABLISHED \
    -j ACCEPT 2>/dev/null || true
```