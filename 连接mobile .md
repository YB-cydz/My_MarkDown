```cmd
sudo nmcli con add type wifi con-name "BUPT-mobile" ifname "wlan0" ssid "BUPT-mobile"  
sudo nmcli con modify "BUPT-mobile" 802-1x.eap peap 802-1x.phase2-auth mschapv2 802-1x.identity "2024210893" 802-1x.password "Zlc2024678!"
sudo nmcli con modify "BUPT-mobile" wifi-sec.key-mgmt wpa-eap
sudo nmcli con up BUPT-mobile
```