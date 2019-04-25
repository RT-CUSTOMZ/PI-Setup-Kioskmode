
# SSH
## Enable SSH on Raspberry Pi
```bash
sudo systemctl enable ssh
sudo systemctl start ssh
```

## SSH on localhost
To not type the passwort each time for ssh make a copy-id on your localhost:
ssh pi@[IP-Address]
ssh-copy-id pi@[IP-Address]

# Setup with ansible

# 1. Install Ansible on your localhost
# 2. Ensure ssh connection with the above command (check with ssh pi@[IP-Address] if you have to type a password)
# 3. Add IP Adress to your inventory (/etc/ansible/hosts)

The Skript can be used for Debian 9 and Raspbian installations 
```bash
ansible-playbook <File_Name> -i <IP-Address-of-PI>
```

# Manual Setup

## PI-Setup-Kioskmode-Campuswoche

### 1.  update packages

```bash
sudo apt-get update

sudo apt-get upgrade
```


### 2.  Configurate pi, wifi credencials, ssh, keyboard layout etc.
```bash
sudo raspi-config
```
choose autologin to desktop

### 3. Install Packages

```bash
sudo apt-get install chromium x11-xserver-utils
sudo apt-get install --no-install-recommends xserver-xorg
sudo apt-get install --no-install-recommends xinit
sudo apt-get install --no-install-recommends raspberrypi-ui-mods lxsession
sudo apt-get install lightdm
sudo apt-get install unclutter
sudo apt-get install xdotool
```

### 4. Edit/create Autostart file:
```bash
mkdir .config/lxsession/LXDE-pi/
nano .config/lxsession/LXDE-pi/autostart
```

### 5. Autostartfile input
```bash
@xscreensaver -no-splash
@xset s off
@xset -dpms
@xset s noblank
@sed -i 's/"exited_cleanly": false/"exited_cleanly": true/' ~/.config/chromium/Default/Preferences
@chromium-browser --noerrdialogs --kiosk http://bilder.campuswoche.de/viewer --incognito
@/home/pi/xauth_root.sh
@/home/pi/autorefresh-chromium.sh
```

* First line disables screensaver,
* second to fourth line disable power management settings and stop screen blanking after a period of inactivity 
* fifth line prevents error messages displaying on the screen in the instance that someone accidentally power cycles the pi without going through the shutdown procedure
* sixth line starts chromium and provides which page to load once it boots without error dialogs and in Kiosk mode

### 7. Autorefresh skript [optional]
Create and open /home/pi/autorefresh-chromium.sh
```bash
sudo nano /home/pi/autorefresh-chromium.sh
```
Input the following:
```bash
#!/bin/bash

#
# also see instructions here: https://www.raspberrypi.org/forums/viewtopic.php?t=178206#p1239241
#
# To make this run with sudo (which is the case when run at boot), execute "xauth_root.sh" before running this script.
#
# xdotools setup instructions found here: http://theembeddedlab.com/tutorials/simulate-keyboard-mouse-events-xdotool-raspberry-pi/
#

# This will only set up the DISPLAY variable for one command
DISPLAY=:0 xdotool key "ctrl+F5"

# This will set up the DISPLAY variable for every command executed on this terminal,
# and child processes spawned from this terminal
export DISPLAY=:0

while true; #create an infinite loop
do
  xdotool key "ctrl+F5" &
  sleep 1200 #refresh time in seconds so 1200 = every 20 min
done
```
### 8. Make File executable
```bash
sudo chmod 755 /home/pi/autorefresh-chromium.sh
```

### 9. Create and open /home/pi/xauth_root.sh
```bash
sudo nano /home/pi/xauth_root.sh
```
Input the following:
```bash
#!/bin/bash

#
# source: https://raspberrypi.stackexchange.com/questions/1719/x11-connection-rejected-because-of-wrong-authentication
#
touch /root/.Xauthority
xauth merge /home/pi/.Xauthority
export XAUTHORITY=/root/.Xauthority
```
## 10. Make File executable
```bash
sudo chmod 755 xauth_root.sh
```
## 11. restart
```bash
sudo reboot
```

