## Setup Flatpak and Arduino IDE installation

```bash
sudo apt update -y

sudo apt install flatpak

flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo

flatpak install flathub cc.arduino.arduinoide
```

## Arduino IDE setup

```
File > Preferences > Settings > Additional Boards Managers URLs:

https://raw.githubusercontent.com/digistump/arduino-boards-index/refs/heads/master/package_digistump_index.json

Then save.
```

```
Tools > Board > Boards Manager:

Digistump AVR Boards

Install. Then change the Board to Digispark (Default 16.5mhz).
```

```
Tool > Programmer:

Micronucleus
```

## Linux User and Driver setup

```bash
sudo adduser $USER dialout
```

```bash
sudo nano /etc/udev/rules.d/49-micronucleus.rules

Copy-paste the following:

# UDEV Rules for Micronucleus boards including the Digispark.
# This file must be placed at:
#
# /etc/udev/rules.d/49-micronucleus.rules    (preferred location)
#   or
# /lib/udev/rules.d/49-micronucleus.rules    (req'd on some broken systems)
#
# After this file is copied, physically unplug and reconnect the board.
#
SUBSYSTEMS=="usb", ATTRS{idVendor}=="16d0", ATTRS{idProduct}=="0753", MODE:="0666"
KERNEL=="ttyACM*", ATTRS{idVendor}=="16d0", ATTRS{idProduct}=="0753", MODE:="0666", ENV{ID_MM_DEVICE_IGNORE}="1"
#
# If you share your linux system with other users, or just don't like the
# idea of write permission for everybody, you can replace MODE:="0666" with
# OWNER:="yourusername" to create the device owned by you, or with
# GROUP:="somegroupname" and mange access using standard unix groups.

Then save (CTRL-O, CTRL-X).

sudo udevadm control --reload-rules
```
