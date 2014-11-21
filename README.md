### Information
This layer provides support for Atmel | SMART Connectivity and SMART microprocessors (AT91 & WILC1000)

For more information about the Atmel | SMART product line see:
* [http://www.atmel.com/products/microcontrollers/arm/](http://www.atmel.com/products/microcontrollers/arm/)
* [http://www.atmel.com/products/wireless/wifi/](http://www.atmel.com/products/wireless/wifi/)

Linux & Open Source on Atmel | SMART:
* [http://www.linux4sam.org](http://www.linux4sam.org)

***

#### Supported SoCs / MACHINE names  
* sama5d3-xplained  
* WILC1000 SD pro  

***

#### Dependencies
This Layer depends on :
* linux-at91
URI : [https://github.com/linux4SC/linux-at91](https://github.com/linux4SC/linux-at91)

* wilc1000 firmware
URI : [https://github.com/linux4SC/wilc_firmware](https://github.com/linux4SC/wilc_firmware)

***

#### Build procedure
Clone buildroot-at91 git repository

    $ git clone https://github.com/linux4SC/buildroot-at91.git  

Checkout buildroot-2013.11-at91

~~~~
$ cd buildroot-at91
$ git checkout buildroot-2013.11-at91 -b my_branch
~~~~

Initialize default configuration

```
$ make sama5d3_xplained_defconfig
```

The default configuration may miss some of services for network system.  
So, they should be included by the following command before building the buildroot

~~~~
$ make menuconfig
~~~~

>* For a station mode, the below option should be included

>>Move to “Target package -> Networking applications -> wpa_supplicant” and check

>><img src="https://github.com/atmchrispark/Image/blob/master/supplicant.jpg" width="533" height="277"/>

>* For AP mode, the below option should be included

>>Move to “Target package -> Networking applications -> hostapd” and check

>><img src="https://github.com/atmchrispark/Image/blob/master/hostapd.jpg" width="533" height="277"/>

>* For DHCP server/client, the below option should be included

>>Move to “Target package -> Networking applications -> dhcp (ISC) and dhcpcd” and check

>>![](https://github.com/atmchrispark/Image/blob/master/dhcp.jpg)

Build core image

    $ make

>Don't use -j option.

>If a qustion is displayed about qt during building, input 'o' and 'yes'.


Built binary path for kernel and rootfs

    $ ./output/images/

>Use prebuilt binary for AT91Bootstrap and U-Boot.  
>[ftp://www.at91.com/pub/demo/linux4sam_4.3/linux4sam-poky-sama5d3_xplained-4.3.zip](ftp://www.at91.com/pub/demo/linux4sam_4.3/linux4sam-poky-sama5d3_xplained-4.3.zip)  

***

#### How to running WILC1000  
This chapter describes how to operate the WILC1000 on the SAMA5D3 Xplained board.

* Recognizing WILC1000 SDIO card module  
SDIO card is recognized during bootup with the following line or equivalent.  


    mmc0: new high speed SDIO card at address 0001  

The following shows how to load the wilc1000 module driver.  
```
Welcome to Buildroot
buildroot login: root
# insmod /lib/modules/3.10.0/kernel/drivers/net/wireless/atmel/wilc1000.ko
IN INIT FUNCTION
*** WILC1000 driver VERSION=[10.0.0] REVISON=[417] FW_VER=[wilc1000_fw.bin] ***
DBG [linux_wlan_device_power: 186]linux_wlan_device_power.. (1)
DBG [linux_wlan_device_detection: 202]linux_wlan_device_detection.. (1)
DBG [linux_sdio_probe: 134]probe function
DBG [linux_sdio_probe: 145]Initializing netdev
DBG [WILC_WFI_WiphyRegister: 4568]Registering wifi device
DBG [WILC_WFI_CfgAlloc: 4515]Allocating wireless device
DBG [WILC_WFI_WiphyRegister: 4634]Successful Registering
DBG [WILC_WFI_WiphyRegister: 4568]Registering wifi device
DBG [WILC_WFI_CfgAlloc: 4515]Allocating wireless device
DBG [WILC_WFI_WiphyRegister: 4634]Successful Registering
```

##### Running as Station Mode  
Station mode is normal Wi-Fi operation that is connecting to a given AP.  
Execute wpa_supplicant with following command on terminal. The wpa_supplicant will automatically perform the background scan with “`ifconfig wlan0 up`” command.  
```
# wpa_supplicant -Dnl80211 -iwlan0 -c/etc/wilc_wpa_supplicant.conf &

DBG [wlan_init_locks: 1779]Initializing Locks ...
DBG [linux_to_wlan: 1850]Linux to Wlan services ...
DBG [wilc_wlan_init: 2280]Initializing WILC_Wlan ...
[wilc sdio]: chipid (001002b0)
[wilc sdio]: has_thrpt_enh3 = 1...
[MMM] malloc before, g_wlan.tx_buffer = 0x0, g_wlan.rx_buffer = 0x0
[mem] tx_buf = 0xcf880000
[MMM] g_wlan.tx_buffer = 0xcf880000
[mem] rx_buf = 0xcf8a0000
[MMM] g_wlan.rx_buffer =0xcf8a0000
DBG [wlan_initialize_threads: 1921]Initializing Threads ...
DBG [wlan_initialize_threads: 1952]Creating kthread for transmission
DBG [linux_wlan_firmware_download: 1384]Downloading Firmware ...
[mem] fw_buf = 0xcf845000
[MMM] dma_buffer = 0xcf845000
DBG [wilc_wlan_firmware_download: 1649]Downloading firmware size = 124540 ...
DBG [wilc_wlan_firmware_download: 1686]Offset = 110304
DBG [wilc_wlan_firmware_download: 1686]Offset = 124540
DBG [linux_wlan_firmware_download: 1391]Freeing FW buffer ...
DBG [linux_wlan_firmware_download: 1392]Releasing firmware
DBG [linux_wlan_firmware_download: 1396]Download Succeeded 
DBG [linux_wlan_start_firmware: 1330]Starting Firmware ...
DBG [linux_wlan_start_firmware: 1341]Waiting for Firmware to get ready ...
ERR [GnrlAsyncInfoReceived: 7559]Wifi driver handler is equal to NULL
DBG [linux_wlan_start_firmware: 1367]Firmware successfully started
...
```

The following commands demonstrate how to scan and connect to the AP.  
```
# wpa_cli -p/var/run/wpa_supplicant ap_scan 1 
# wpa_cli -p/var/run/wpa_supplicant add_network 
# wpa_cli -p/var/run/wpa_supplicant set_network 0 ssid '"User_AP"' 
# wpa_cli -p/var/run/wpa_supplicant set_network 0 key_mgmt NONE 
# wpa_cli -p/var/run/wpa_supplicant select_network 0
```
>Please change “User_AP” with SSID of desired AP.  

Start up dhcp client  

    #dhcpcd wlan0  

Check the following command to see the connection is established.  
```
# wpa_cli status

bssid=88:9b:39:f3:d0:4d
ssid=User_AP
id=0
mode=station
pairwise_cipher=NONE
group_cipher=NONE
key_mgmt=NONE
wpa_state=COMPLETED
ip_address=192.168.43.2
address=00:80:c2:b3:d7:4d
```  
######* Create `wilc_wpa_supplicant.conf` file  
Create `/etc/wilc_wpa_supplicant.conf` like the below:  
```  
ctrl_interface=/var/run/wpa_supplicant  
update_config=1  
```  
> Reference : [http://w1.fi/cgit/hostap/plain/wpa_supplicant/wpa_supplicant.conf](http://w1.fi/cgit/hostap/plain/wpa_supplicant/wpa_supplicant.conf)  

##### Running as AP Mode  
The following command demonstrates how to execute the hostapd.  
```
# hostapd /etc/wilc_hostapd.conf -B &

DBG [WILC_WFI_change_virt_intf: 3380]Wireless interface name =wlan0
DBG [WILC_WFI_change_virt_intf: 3389]Changing virtual interface, enable scan
DBG [WILC_WFI_change_virt_intf: 3607]Interface type = NL80211_IFTYPE_AP 3
(WILC_Uint32)priv->hWILCWFIDrv[0]
DBG [WILC_WFI_change_virt_intf: 3617]Downloading AP firmware
DBG [WILC_WFI_add_virt_intf: 4291]Adding monitor interface[cf079800]
DBG [WILC_WFI_add_virt_intf: 4298]Monitor interface mode: Initializing mon interface virtual device driver
DBG [WILC_WFI_add_virt_intf: 4299]Adding monitor interface[cf079800]
DBG [WILC_WFI_add_virt_intf: 4303]Setting monitor flag in private structure
MAC OPEN[cf079800]
DBG [WILC_WFI_InitHostInt: 4681]Host[cf079800][cf076ec0]
DBG [mac_open: 2380]*** re-init ***
DBG [wlan_init_locks: 1779]Initializing Locks ...
DBG [linux_to_wlan: 1850]Linux to Wlan services ...
DBG [wilc_wlan_init: 2280]Initializing WILC_Wlan ...
[wilc sdio]: chipid (001002b0)
...
```  

>Refer to wilc_hostapd.conf for the details of AP settings  

Updated the dhcpd.conf  like the following.  
   * /etc/dhcp/dhcpd.conf  
```
ddns-update-style none;
default-lease-time 600;
max-lease-time 7200;

option subnet-mask 255.255.255.0;
option domain-name-servers 168.126.63.1, 164.124.101.2; // DNS Server IP
option domain-name “sample.example”;                    // domain name

subnet 192.168.0.0 netmask 255.255.255.0 {
   range 192.168.0.100 192.168.0.110;                   // range ip
   option broadcast-address 192.168.0.255;
   option routers 192.168.0.1;                          // gateway ip
}
Log-facility local7;
```
>Each values must be modified to fit the test environment.  

Copy the followings in the `/etc/` directory  

    #cp /etc/dhcp/dhcpd.conf /etc/

Update the S80dhcp-server like the following.  
   * /etc/init.d/S80dhcp-server  


    INTERFACES=”wlan0”
    
Set the IP to gateway.  

    #ifconfig wlan0 192.168.0.1
    
>Gateway IP is defined in the `dhcpd.conf`.  

Start up dhcp server  

    #/etc/init.d/S80dhcp-server start  
    
######* Create `wilc_hostapd.conf` file  
Create `/etc/wilc_hostapd.conf` like the below:  
```  
interface=wlan0  
driver=nl80211  
ctrl_interface=/var/run/hostapd  
ssid=SoftAP  
dtim_period=2  
beacon_int=100  
channel=1  
hw_mode=g  
max_num_sta=8  
ap_max_inactivity=300  
```  
> Reference : [http://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf](http://w1.fi/cgit/hostap/plain/hostapd/hostapd.conf)   

Now you can launch PC or Smartphone to connect to WILC1000 device, which is running as AP mode.  
