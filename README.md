#### /etc/systemd/system/powertop.service

```ini
[Unit]
Description=Powertop tunings with Human Interface Device adjustment

[Service]
Type=oneshot
RemainAfterExit=yes
ExecStart=/path/to/powertune.sh

[Install]
WantedBy=multi-user.target
```

#### powertune.sh
```bash
#!/bin/bash
powertop --auto-tune
HIDDEVICES=$(ls /sys/bus/usb/drivers/usbhid | grep -oE '^[0-9]+-[0-9\.]+' | sort -u)
for i in $HIDDEVICES; do
  echo -n "Enabling " | cat - /sys/bus/usb/devices/$i/product
  echo 'on' > /sys/bus/usb/devices/$i/power/control
done
```

Based on https://askubuntu.com/questions/678779/powertop-auto-tune-without-messing-with-usb-and-touchpad
