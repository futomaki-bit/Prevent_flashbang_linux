# Testing in Progress

### Testing on Dell Inspiron 15 (3525) laptop with Ryzen 5 5625U
brightness value located at:
`/sys/class/backlight/amdgpu_bl1/brightness`

## Create script
`sudo nano /usr/local/bin/apply_brightness.sh`
```sh
#!/bin/bash

BRIGHTNESS_FILE="/sys/class/backlight/amdgpu_bl1/brightness"

# Read the current brightness
current_brightness=$(cat $BRIGHTNESS_FILE)

# Apply the current brightness value
echo $current_brightness | sudo tee $BRIGHTNESS_FILE
```
`sudo chmod +x /usr/local/bin/apply_brightness.sh`

## Create Service
`sudo nano /etc/systemd/system/apply-brightness.service`
```sh
[Unit]
Description=Apply current brightness on suspend/resume
After=suspend.target

[Service]
Type=oneshot
ExecStart=/usr/local/bin/apply_brightness.sh

[Install]
WantedBy=sleep.target suspend.target hibernate.target hybrid-sleep.target
```
```
sudo systemctl daemon-reload
sudo systemctl enable apply-brightness.service
sudo systemctl start apply-brightness.service
```
