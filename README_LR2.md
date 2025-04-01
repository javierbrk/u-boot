# mt7621_rfb/mt7621_nand_rfb

U-Boot for the MediaTek MT7621 boards

Quick Start

    Get the DDR initialization binary blob

    Build U-Boot

## Get the DDR initialization binary blob

Download one from:

        https://raw.githubusercontent.com/mtk-openwrt/mt7621-lowlevel-preloader/master/mt7621_stage_sram.bin

        https://raw.githubusercontent.com/mtk-openwrt/mt7621-lowlevel-preloader/master/mt7621_stage_sram_noprint.bin

	-There is a downloaded version in this branch

mt7621_stage_sram_noprint.bin has removed all output logs. To use this one, download and rename it to mt7621_stage_sram.bin

Put the binary blob to the u-boot build directory.

## Build U-Boot
```BASH
    $ make O=build librerouter2_defconfig
    $ cp mt7621_stage_sram.bin ./build/mt7621_stage_sram.bin
    $ make O=build
```

VERY IMPORTANT
Burn the u-boot-mt7621.bin to the SPI FLASH

# Upload the new u-boot
## config env variables in old u-boot
First configure these environment variables in u-boot
You need to configure the bootargs so the Linux image can boot.

setenv bootcmd 'setenv bootargs console=ttyS0,115200 rootfstype=squashfs,jffs2; bootm ${fw1_addr}'
setenv fw1_addr 'bfc50000'
setenv baudrate 115200

## Upload the new u-boot
configure a tftp server in your computer

### Default with old uboot
download the [binary](https://github.com/javierbrk/u-boot/blob/2b5c8112440268df1cd81697e67f10be1b6f2bf1/build/u-boot-mt7621.bin)
start uboot .. press option 9
configure host IP, server IP and binary name in the console.
wait

VERY IMPORTANT 
Burn the u-boot-mt7621.bin to the SPI FLASH


### From "new" u-boot

```
=> sf probe
SF: Detected w25q256 with page size 256 Bytes, erase size 4 KiB, tofwtal 32 MiB
=> 
```
1. First, you've already executed `sf probe` which initializes the SPI flash interface, so that's good.

2. Next, you'll need to load the new U-Boot image into memory. You have several options:

   a. If you have TFTP server set up on your network:
   ```
   setenv ipaddr 192.168.1.x    # Set router IP address
   setenv serverip 192.168.1.y  # Set TFTP server IP
   tftpboot 0x80100000 u-boot-mt7621.bin
   ```
:::warning 
***Not Tested!***

   b. If you can use USB storage:
   ```
   usb start
   fatload usb 0:1 0x80100000 u-boot-mt7621.bin
   ```
:::

3. Once the image is loaded to memory, you need to erase the flash sector where U-Boot resides:
   ```
   sf erase 0 0x30000
   ```
4. Then write the new U-Boot image to flash:
   ```
   sf write 0x80100000 0 ${filesize}
   ```
5. Finally, reset the router:
   ```
   reset
   ```

