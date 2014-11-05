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

Build core image

    $ make

>Don't use -j option.

>If a qustion is displayed about qt during building, input 'o' and 'yes'.


Built binary path for kernel and rootfs

    $ ./output/images/

***

>Use prebuilt binary for AT91Bootstrap and U-Boot.

>[ftp://www.at91.com/pub/demo/linux4sam_4.3/linux4sam-poky-sama5d3_xplained-4.3.zip](ftp://www.at91.com/pub/demo/linux4sam_4.3/linux4sam-poky-sama5d3_xplained-4.3.zip)
