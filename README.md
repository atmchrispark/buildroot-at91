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

    $ cd buildroot-at91
    $ git checkout buildroot-2013.11-at91 -b my_branch

Initialize default configuration

    $ make sama5d3_xplained_defconfig

The default configuration may miss some of services for network system.

So, they should be included by the following command before building the buildroot

    $ make menuconfig
    
>For a station mode, the below option should be included

>>Move to “Target package -> Networking applications -> wpa_supplicant” and check

>><img src="https://github.com/atmchrispark/Image/blob/master/supplicant.jpg" width="533" height="277"/>

>For AP mode, the below option should be included

>>Move to “Target package -> Networking applications -> hostapd” and check

>><img src="https://github.com/atmchrispark/Image/blob/master/hostapd.jpg" width="533" height="277"/>

>For DHCP server/client, the below option should be included

>>Move to “Target package -> Networking applications -> dhcp (ISC) and dhcpcd” and check

>>![](https://github.com/atmchrispark/Image/blob/master/dhcp.jpg)

Build core image

    $ make

![checkmark](https://github.com/atmchrispark/Image/blob/master/custion.png)Don't use -j option.

![checkmark](https://github.com/atmchrispark/Image/blob/master/custion.png)If a qustion is displayed about qt during building, input 'o' and 'yes'.


Built binary path for kernel and rootfs

    $ ./output/images/

***

>Use prebuilt binary for AT91Bootstrap and U-Boot.

>[ftp://www.at91.com/pub/demo/linux4sam_4.3/linux4sam-poky-sama5d3_xplained-4.3.zip](ftp://www.at91.com/pub/demo/linux4sam_4.3/linux4sam-poky-sama5d3_xplained-4.3.zip)
