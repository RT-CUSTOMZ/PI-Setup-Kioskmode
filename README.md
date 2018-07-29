# PI-Setup-Kioskmode-Campuswoche

## 1.  update packages

```bash
sudo apt-get update

sudo apt-get upgrade
```


## 2.  configurate pi, wifi credencials, ssh, keyboard layout etc.
```bash
sudo raspi-config
```
choose autologin to desktop

## 3. install chromium and x11

```bash
sudo apt-get install chromium x11-xserver-utils
```

## 4. install unclutter (to remove cursor from screen)
```bash
sudo apt-get install unclutter
```




## 8. stuff
sudo apt-get install --no-install-recommends xserver-xorg
sudo apt-get install --no-install-recommends xinit
sudo apt-get install --no-install-recommends raspberrypi-ui-mods lxsession
sudo apt-get install lightdm

## 5. edit/create file:
```bash
mkdir .config/lxsession/LXDE/
nano .config/lxsession/LXDE/autostart
```

## 6. put into file: 
```bash
@xscreensaver -no-splash
@xset s off
@xset -dpms
@xset s noblank
@sed -i 's/"exited_cleanly": false/"exited_cleanly": true/' ~/.config/chromium/Default/Preferences
@chromium --noerrdialogs --kiosk http://www.42volt.de --incognito
```

* First line disables screensaver,
* second to fourth line disable power management settings and stop screen blanking after a period of inactivity 
* fifth line prevents error messages displaying on the screen in the instance that someone accidentally power cycles the pi without going through the shutdown procedure
* sixth line starts chromium and provides which page to load once it boots without error dialogs and in Kiosk mode

## 5. save with `CTRL + O` and exit with `CTRL + X`

## 9. restart
```bash
sudo reboot
```
ssh-copy-id pi@192.168.120.232


