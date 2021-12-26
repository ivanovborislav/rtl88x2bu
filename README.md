-----

## rtl88x2bu (88x2bu.ko)

## Realtek RTL88x2BU Wireless Lan Driver for Linux

- v5.13.1 (20210702)
- Based on EW-7822UTC Linux Driver (Version : 1.0.2.2) 2021-10-26
- Support Kernel: 3.8 - 5.11 (Realtek)
- Support up to Kernel 5.15

## Specification

- Supported interface modes:
  * IBSS
  * managed
  * AP
  * monitor
  * P2P-client
  * P2P-GO
  * [Concurrent mode plus virtual interface control](https://github.com/ivanovborislav/document/blob/main/rtl88x2bu_concurrent_mode.md)
- Packet injection
- TX power control
- LED control
- Power saving control
- Driver debug log level control
- DFS channels control
- VHT control
- Security:
  * WEP 64/128-bit, WPA, WPA2, and WPA3

## Supported adapters

- ASUS USB-AC53 Nano
- ASUS USB-AC54 B1
- ASUS USB-AC55 B1
- ASUS USB-AC57
- ASUS USB-AC58
- D-Link DWA-181 rev A1
- D-Link DWA-182 rev D1
- Edimax EW-7822ULC
- Edimax EW-7822UNC
- Edimax EW-7822UTC
- Hawking HW12ACU
- Linksys WUSB6400M
- Netgear A6150
- Simplecom NW621 AC1200
- TP-LINK Archer T3U
- TP-LINK Archer T3U Plus
- TP-LINK Archer T4U v3
- TRENDnet TEW-808UBM
- Realtek 8822BU Wireless LAN 802.11ac USB NIC
- Realtek 8812BU Wireless LAN 802.11ac USB NIC
- WN8602L Wireless LAN 802.11ac USB NIC
- CC&C 433Mbps Wireless USB2.0 Adapter
- Dacota Platinum AC1200 USB 2.0 Wireless Adapter
- Dacota Platinum AC1200 USB 3.0 Wireless Adapter

## Tested Linux Distros and Tools

Successful tested injection `aireplay-ng -9 <interface>`, deauthentication `aireplay-ng -0 1 -a <bssid> <interface>`.

Successful hcxdumptool driver test `hcxdumptool -i <interface> --check_driver` and capture packets `hcxdumptool -i <interface> -o test.pcapng --filterlist_ap=targets.lst --filtermode=2 --enable_status=15` `(hcxdumptool -i <interface> -o test.pcapng --filterlist=targets.lst --filtermode=2 --enable_status=15)`.

Connecting to AP with WPA3 (WPA3-SAE) authentication method (IMPORTANT: Network Manager, is not support WPA3 on this driver. Connecting with wpa_supplicant).

Start WPA3 (WPA3-SAE) SoftAP.

TX power control tested with SoftAP mode and OpenWrt firmware (OpenWrt 21.02.0 r16279-5cc0535800, Channel Analysis Feature).

- Ubuntu 21.10 (kernel 5.15)
  * wpa_supplicant v2.10-devel-hostap_2_9-2398-g8a54c252a
  * hostapd v2.10-devel-hostap_2_9-2398-g8a54c252a
  * Aircrack-ng 1.6
  * hcxdumptool 6.2.4

- Kali Linux 2021.3a (kernel 5.14)
  * Aircrack-ng 1.6
  * hcxdumptool 5.2.2
  * wpa_supplicant v2.10-devel-hostap_2_9-2433-g8d881d942

- Ubuntu 21.10 (kernel 5.13)
  * Aircrack-ng 1.6
  * hcxdumptool 6.2.0
  * hostapd v2.10-devel-hostap_2_9-2398-g8a54c252a
  * wpa_supplicant v2.10-devel-hostap_2_9-2398-g8a54c252a

- Ubuntu 21.04 (kernel 5.11)
  * Aircrack-ng 1.6
  * hcxdumptool 6.1.6
  * wpa_supplicant v2.10-devel-hostap_2_9-2374-g9ef8491d9

- Kali Linux 2020.3 (kernel 5.9)
  * Aircrack-ng 1.6
  * hcxdumptool 6.1.4
  * wpa_supplicant v2.10-devel-hostap_2_9-2379-g4775a5f82

- Linux Mint 20.2 MATE (kernel 5.4)
  * wpa_supplicant v2.10-devel-hostap_2_9-2486-gbb6fa62b3
  * hostapd v2.10-devel-hostap_2_9-2486-gbb6fa62b3
  * hcxdumptool 6.1.6
  * Aircrack-ng 1.6

- Kali Linux 2019.1a (kernel 4.19)
  * Aircrack-ng 1.5.2
  * hcxdumptool 5.1.7
  * wpa_supplicant v2.10-devel-hostap_2_9-2433-g8d881d942

- Ubuntu 13.04 (kernel 3.8)
  * Aircrack-ng 1.5.2
  * hcxdumptool 5.1.4

- Ubuntu 12.10 (kernel 3.5)
  * Aircrack-ng 1.5.2
  * hcxdumptool 4.2.1

- Ubuntu 12.04 (kernel 3.4)
  * Aircrack-ng 1.5.2
  * hcxdumptool 4.2.1

- Ubuntu 11.10 (kernel 3.0)
  * Aircrack-ng 1.5.2
  * hcxdumptool 4.2.1

## HOW TO

### Install

Download source:

```
git clone https://github.com/ivanovborislav/rtl88x2bu.git
cd rtl88x2bu
```

Install missing packages:

```
sudo apt-get install bc build-essential
```

Install linux headers:

```
sudo apt-get install linux-headers-$(uname -r)
```

or

```
apt-cache search linux-headers
sudo apt-get install linux-headers-5.14.0-kali4-amd64 (for example)
apt-cache search linux-image
sudo apt-get install linux-image-5.14.0-kali4-amd64 (for example)
```

Compile:

```
make
sudo make install
```

### Monitor mode

```
sudo airmon-ng check kill
sudo ip link set <interface> down
sudo iw dev <interface> set type monitor
sudo ip link set <interface> up
```

### Managed mode

```
sudo ip link set <interface> down
sudo iw dev <interface> set type managed
sudo ip link set <interface> up
sudo systemctl restart NetworkManager (sudo service network-manager restart)
```

### TX power control

Note: Set TX power before start SoftAP mode. `...set txpower fixed 3000 = txpower 30.00 dBm`.

```
sudo iw dev <interface> set txpower fixed 3000
```

### Driver options

#### Change driver options during inserting driver module

Remove (unload) a module from the Linux kernel.
```
sudo rmmod /lib/modules/$(uname -r)/kernel/drivers/net/wireless/88x2bu.ko
```

Insert (load) a module into the Linux kernel.
```
sudo insmod /lib/modules/$(uname -r)/kernel/drivers/net/wireless/88x2bu.ko rtw_ips_mode=1 rtw_drv_log_level=4 rtw_power_mgnt=2 rtw_led_ctrl=1
```

#### Change driver options loading from file

Create a file `88x2bu.conf` containing `options 88x2bu rtw_ips_mode=1 rtw_drv_log_level=4 rtw_power_mgnt=2 rtw_led_ctrl=1`.
Copy a file to `/etc/modprobe.d/` directory.

```
sudo cp -f 88x2bu.conf /etc/modprobe.d
```

Power saving control.

IPS (Inactive Power Saving) Function, rtw_ips_mode=
```
0:Disable IPS
1:Enable IPS (default)
```

LPS (Leisure Power Saving) Function, rtw_power_mgnt=
```
0:Disable LPS
1:Enable LPS
2:Enable LPS with clock gating (default)
```

Driver debug log level control, rtw_drv_log_level=
```
0:_DRV_NONE_
1:_DRV_ALWAYS_
2:_DRV_ERR_
3:_DRV_WARNING_
4:_DRV_INFO_ (default)
5:_DRV_DEBUG_
6:_DRV_MAX_
```

Driver LED control, rtw_led_ctrl=
```
0:led off
1:led blink (default)
2:led on
```

Driver DFS channels control, rtw_dfs_region_domain=
```
0:NONE
1:FCC
2:MKK
3:ETSI
```

Driver VHT control, rtw_vht_enable=
```
0:disable
1:enable (default)
2:force auto enable
```

### Connecting with wpa_supplicant

Example wpa_supplicant.conf with WPA3-Personal (WPA3-SAE).

```
update_config=1
ctrl_interface=/var/run/wpa_supplicant
country=EN
p2p_no_group_iface=1
sae_groups=19 20 21

network={
    ssid="WPA3"
    proto=RSN
    key_mgmt=SAE
    pairwise=CCMP
    group=CCMP
    ieee80211w=2
    psk="1234567890"
}
```

Now start...
```
sudo systemctl stop NetworkManager
sudo killall wpa_supplicant
sudo wpa_supplicant -B -i <interface> -c wpa_supplicant.conf
sudo dhclient <interface>
```

### Start SoftAP mode

Example hostapd.conf with WPA3-Personal (WPA3-SAE) 2.4GHz.

```
driver=nl80211
ctrl_interface=/var/run/hostapd
ctrl_interface_group=0
ssid=WPA3
country_code=EN
hw_mode=g
channel=6
beacon_int=100
dtim_period=1
max_num_sta=16
rts_threshold=2347
fragm_threshold=2346
ignore_broadcast_ssid=0
wmm_enabled=1
ieee80211n=1
ht_capab=[RXLDPC][HT40-][SHORT-GI-20][SHORT-GI-40][RX-STBC1][MAX-AMSDU-7935][DSSS_CCK-40]

auth_algs=1
wpa=2
wpa_passphrase=1234567890
wpa_key_mgmt=SAE
wpa_pairwise=CCMP
rsn_pairwise=CCMP
ieee80211w=2
sae_groups=19 20 21
sae_require_mfp=1
````

Example hostapd.conf with WPA3-Personal (WPA3-SAE) 5GHz.

CAUTION: Allow width: 80 MHz, `insmod 88x2bu.ko rtw_vht_enable=2`.

```
driver=nl80211
ctrl_interface=/var/run/hostapd
ctrl_interface_group=0
ssid=WPA3_5GHz
country_code=EN
hw_mode=a
channel=36
beacon_int=100
dtim_period=1
max_num_sta=16
rts_threshold=2347
fragm_threshold=2346
ignore_broadcast_ssid=0
wmm_enabled=1
ieee80211n=1
ht_capab=[RXLDPC][HT40+][SHORT-GI-20][SHORT-GI-40][MAX-AMSDU-7935][DSSS_CCK-40]
ieee80211ac=1
vht_capab=[MAX-MPDU-11454][RXLDPC][SHORT-GI-80][TX-STBC-2BY1][RX-STBC1][SU-BEAMFORMEE][MU-BEAMFORMEE][HTC-VHT][MAX-A-MPDU-LEN-EXP7]
vht_oper_chwidth=1
vht_oper_centr_freq_seg0_idx=42

auth_algs=1
wpa=2
wpa_passphrase=1234567890
wpa_key_mgmt=SAE WPA-PSK
wpa_pairwise=CCMP
rsn_pairwise=CCMP
ieee80211w=2
sae_groups=19 20 21
sae_require_mfp=1
```

Now start...
```
sudo killall hostapd
sudo hostapd -i <interface> hostapd.conf
```

### [Kali NetHunter Kernel module Samsung Galaxy S5](https://github.com/ivanovborislav/document/blob/main/NetHunter_Kernel_module_Samsung_Galaxy_S5.md)

## Test devices

- TP-Link Archer T3U V1

- TP-Link Archer T4U V3

- Linksys WRT1200AC V2
  * OpenWrt 21.02.0 r16279-5cc0535800 / LuCI openwrt-21.02 branch git-21.231.26241-422c175

```
config wifi-iface 'default_radio1'
	option device 'radio1'
	option network 'lan'
	option mode 'ap'
	option macaddr '30:23:03:XX:XX:XX'
	option ssid 'WPA3'
	option encryption 'sae'
	option key '1234567890'
	option ieee80211w '2'
```

-----
