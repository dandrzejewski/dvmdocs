## Raspberry Pi Preparation

Some extra notes for those who are using the Raspberry Pi, default Raspbian OS or Debian OS installations. You will not be able to flash or access the STM32 modem unless you do some things beforehand.

1. Disable the Bluetooth services. Bluetooth will share the GPIO serial interface on `/dev/ttyAMA0`. On Rasbian OS or Debian OS, this is done by: `sudo systemctl disable bluetooth` then adding `dtoverlay=disable-bt` to `/boot/config.txt`.
1. The default Rasbian OS and Debian OS will have a getty instance listening on `/dev/ttyAMA0`. This can conflict with the STM32, and is best if disabled. On Rasbian OS or Debian OS, this is done by: `systemctl disable serial-getty@ttyAMA0.service`
1. On Debian Bookworm-based builds of Raspian OS, the getty instance on `/dev/ttyAMA0` gets rebuilt on boot via a systemd generator, even if you've already disabled it.  You'll need to disable this generator with: `sudo systemctl mask serial-getty@ttyAMA0.service`
1. There's a default boot option which is also listening on the GPIO serial interface. This **must be disabled**. Open the `/boot/cmdline.txt` file in your favorite editor (vi or pico) and remove the `console=serial0,115200` part.

The steps above can be done by the following commands:

```shell
sudo systemctl disable bluetooth.service serial-getty@ttyAMA0.service
sudo systemctl mask serial-getty@ttyAMA0.service
grep '^dtoverlay=disable-bt' /boot/config.txt || echo 'dtoverlay=disable-bt' | sudo tee -a /boot/config.txt
sudo sed -i 's/^console=serial0,115200 *//' /boot/cmdline.txt
```

After finishing these steps, reboot.
