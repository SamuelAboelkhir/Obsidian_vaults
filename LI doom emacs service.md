---
tags: 
- services
- LI
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Linux MOC|Back to Linux MOC]] 
# User services
- Not all services need to live under `/etc/systemd/system/` as this path runs as root, and is meant for system services that require such privileages [[LI Creating a Service]]
- For a simpler user service, such as an emacs daemon, the service can go under `~/.config/systemd/user/` instead
```
[Unit]
Description=Doom Emacs daemon

[Service]
ExecStart=/usr/bin/emacs --init-directory /home/blackdovah/.config/emacs --fg-daemon=doom
Restart=on-failure

[Install]
WantedBy=default.target
```