---
tags: 
- hypr
- LI
MOC: Technology
---
[[_0000 Home|Home]] | [[_0001 Linux MOC|Back to Linux MOC]] 

# Create a desktop app entry
- Any executable can be turned into an application entry that will be visible and runnable under the default app menu of the system, such as the apps section of the omarchy menu
- To do so, we need an entry under `~/.local/share/applications`
- For example:
```desktop
[Desktop Entry]
Name=Doom Emacs
Exec=/usr/bin/emacsclient --create-frame --alternate-editor="" --socket-name=doom %F
Terminal=false
Type=Application
Icon=emacs
Comment=Doom Emacs
Categories=Utility;Development;TextEditor;
StartupWMClass=Emacs
```
- We may also need a service to autostart on boot, such as a script or daemon
- For that, we have a very similar syntax, basically the same, but the location differs `~/.config/autostart`
- Example:
```desktop
[Desktop Entry]
Name=DoomEmacs
Exec=/home/blackdovah/.config/emacs/bin/doom-emacs --fg-daemon=doom
Terminal=false
Type=Application
Icon=emacs
Comment=Doom Emacs
Categories=Utility;Development;TextEditor;
StartupWMClass=Emacs
```
- Note that autostart will start the indicated apps when the graphical desktop session starts
- This is different from systemd services that can start launchs processes as services when the user system starts [[LI VM firewall rules service]]