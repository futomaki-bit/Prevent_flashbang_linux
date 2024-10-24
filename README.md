# Testing in Progress

### Testing on Dell Inspiron 15 (3525) laptop with Ryzen 5 5625U
brightness value located at:
`/sys/class/backlight/amdgpu_bl1/brightness`

`sudo nano /usr/local/bin/get_brightness.sh`
```sh
#!/bin/bash

BRIGHTNESS_FILE="/sys/class/backlight/amdgpu_bl1/brightness"

# Read the current brightness
current_brightness=$(cat $BRIGHTNESS_FILE)

# Increment by 1
new_brightness=$((current_brightness + 1))

# Write the new brightness value
echo $new_brightness | sudo tee $BRIGHTNESS_FILE
```
`sudo chmod +x /usr/local/bin/get_brightness.sh`

`sudo nano /etc/systemd/system/set-brightness.service`
```sh
[Unit]
Description=Increment brightness on resume

[Service]
Type=oneshot
ExecStart=/usr/local/bin/increment_brightness.sh

[Install]
WantedBy=suspend.target
```
`sudo systemctl daemon-reload`

`sudo systemctl enable set-brightness.service`
