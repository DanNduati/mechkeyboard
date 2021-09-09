# Mechanical keyboard Firmware
Firmware for Nordic MCUs used for clients custom mechanical Keyboard, contains precompiled .hex files, as well as sources buildable with the Nordic SDK

## Folder structure
	.
	├── docs        # Documentation directory
	├── firmware    # The keyboard and receiver firmware
	├── images      # Images directory
	├── kbfirmware  # kbfirmware directory
	└── README.md
## Install dependencies

Tested on Ubuntu 16.04.2, but should be able to find alternatives on all distros. 

```
sudo apt install openocd gcc-arm-none-eabi
```

## Download Nordic SDK

Nordic does not allow redistribution of their SDK or components, so download and extract from their site:

https://www.nordicsemi.com/Products/Development-software/nRF5-SDK/Download#infotabs

Firmware written and tested with version 11

```
unzip nRF5_SDK_11.0.0_89a8197.zip  -d nRF5_SDK_11
cd nRF5_SDK_11
```

## Toolchain set-up

A cofiguration file that came with the SDK needs to be changed. Assuming you installed gcc-arm with apt, the compiler root path needs to be changed in /components/toolchain/gcc/Makefile.posix, the line:
```
GNU_INSTALL_ROOT := /usr/local/gcc-arm-none-eabi-4_9-2015q1
```
Replaced with:
```
GNU_INSTALL_ROOT := /usr/
```

## Clone repository
Inside nRF5_SDK_11/
```
git clone git@github.com:DanNduati/mechkeyboard.git
```

Plug in the stlink v2 programmer.

## OpenOCD server
The programming header in the doc should be: 
```
SWCLK
SWDIO
GND
3.3V
```

Launch a debugging session with:
```
openocd -f firmware/nrf-stlinkv2.cfg
```
Should give you an output ending in:
```
Info : nrf51.cpu: hardware has 4 breakpoints, 2 watchpoints
```
If the pcb is fine


## Manual programming
From the factory, these chips need to be erased:
```
echo reset halt | telnet localhost 4444
echo nrf51 mass_erase | telnet localhost 4444
```
From there, the precompiled binaries can be loaded:
```
echo reset halt | telnet localhost 4444
echo flash write_image `readlink -f precompiled-basic-left.hex` | telnet localhost 4444
echo reset | telnet localhost 4444
```

## Automatic make and programming scripts
To use the automatic build scripts:
```
cd firmware/keyboard/source/
./program.sh
```
An openocd session should be running in another terminal, as this script sends commands to it.

## Pinout table and Wiring Diagram 
|             |     |     |     |     |     |     |     |     |     |     |     |     |     |     |
|:-----------:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|:---:|
|   Row Pins  | P01 | P02 | P03 | P04 | P05 |     |     |     |     |     |     |     |     |     |
| Column Pins | P06 | P07 | P08 | P09 | P10 | P11 | P12 | P13 | P14 | P15 | P16 | P17 | P18 | P19 |

<img scr="/images/wiring_diagram.png"></img>
