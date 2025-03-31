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
