# sync_brightness
Sync the value of the backlight brightness between the AMD and NVIDIA interface

Welcome to this repository!
This script is SPECIFICALLY meant to read brightness changes from /sys/class/backlight/nvidia_0/brightness and mirror them in either /sys/class/backlight/amdgpu_bl1/brightness or /sys/class/backlight/amdgpu_bl2/brightness.
To know if you need this script, check the following criteria:
 - You are using a laptop.
 - You have an AMD AND an NVIDIA GPU.
 - When you press the brightness keys, a brightness slider appears, but the actual screen's brightness does not change.
 - /sys/class/backlight/nvidia_0/brightness AND one of /sys/class/backlight/amdgpu_bl1/brightness or /sys/class/backlight/amdgpu_bl2/brightness exist (1)
 - The file /sys/class/backlight/nvidia_0/brightness changes when you use the brightness keys (2)
 - If you write to /sys/class/backlight/amdgpu_bl1/brightness or /sys/class/backlight/amdgpu_bl2/brightness (the one that exists), the screen's brightness changes (3)

To check (1), run the following command: `ls /sys/class/backlight/`. You will get a list of existing backlight interfaces.
To check (2), run the following command: `cat /sys/class/backlight/nvidia_0/brightness`, spam the brightness keys, then run the command again. You should get a different number the second time.
To check (3), run the following command: `sudo echo 50 >> /sys/class/backlight/amdgpu_bl*/brightness`. It will be immediately obvious if this works, as the screen should become significantly dimmer.

This script was created by Caedorus. I consider it too small to fall under most copyright laws. Basically, use it as you see fit. It was made and tested for Linux Mint 22.2 ("Zara") on a Legion Pro 5 16AFR10; it almost certainly works on most even remotely similar distributions. No promises though.

# Install

To install these modifications, put fix_brightness.service in /etc/systemd/system; put sync_backlights in /root/.scripts (you'll have to make it); run as root:
```chown root:root /etc/systemd/system/fix_brightness.service && chown root:root /root/.scripts/sync_backlights
systemctl daemon-reload
systemctl enable fix_brightness.service
systemctl start fix_brightness.service
