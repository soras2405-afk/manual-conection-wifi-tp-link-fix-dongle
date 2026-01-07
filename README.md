# manual-conection-wifi-tp-link-fix-dongle
fix wifi dongle 

. Install iw (cek & scan WiFi)
sudo apt update
sudo apt install iw -y

2. Install dhclient (ambil IP)
sudo apt install isc-dhcp-client -y

3. iw dev < fine Interface example wlxccbabdb47753 >
4. wpa_passphrase "name wifi" "pasword wifi "
5. sudo nano /etc/wpa_supplicant/wpa-rtl8188.conf
   
 copy all
 
 ctrl_interface=DIR=/run/wpa_supplicant GROUP=netdev
update_config=1
country=ID ( change country )

network={
    ssid="Name wifi"
    #psk="password wifi"
    psk=HASIL_HASH_64_KARAKTER < change scan with wpa_passphrase "name wifi" "password wifi" >
    scan_ssid=1
    priority=10
}

 or 

 wpa_passphrase "name wifi" "password wifi" | sudo tee /etc/wpa_supplicant/wpa-rtl8188.conf
6. konect wifi 
sudo ip link set < Interface example wlxccbabdb47753 > up
sudo wpa_supplicant -B -i <example wlxccbabdb47753 >-c /etc/wpa_supplicant/wpa-rtl8188.conf
sudo dhclient <exsample wlxccbabdb47753 >

7. auto conect wifi after shutdown
8.  sudo nano /etc/systemd/system/wifi-manual.service
9. copy all 
[Unit]
Description=Manual WiFi Auto Connect
After=network.target

[Service]
Type=simple
ExecStart=/sbin/wpa_supplicant -i < Interface example wlxccbabdb47753 > -c /etc/wpa_supplicant/wpa-rtl8188.conf
ExecStartPost=/sbin/dhclient < Interface example wlxccbabdb47753 >
Restart=always

[Install]
WantedBy=multi-user.target

10. 
sudo systemctl daemon-reload
sudo systemctl enable wifi-manual.service

11. reboot
   




