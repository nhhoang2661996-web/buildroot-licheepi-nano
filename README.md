# Buildroot LicheePi Nano

##  Overview
this repository contains Buildroot configuration for the LicheePi Nano board

## Hardware
- Sipeed Lichee Nano F1C100s ARM926EJS Linux Development Board
- MicroSD card 16gb
## Development Environment
- Host OS: Windows 10
- Linux environment: WSL2 (Ubuntu 22.04
### Tool(UART Debug):
- PuTTY (Windows)
## How to Build
1. **Clone Buildroot repository:**
   ```bash
   git clone https://github.com/buildroot/buildroot.git
   cd buildroot
   ```
2. **Apply custom configuration:**
   ```bash
   make sipeed_licheepi_nano_defconfig
   ```
   
   The file `board/genimage_sd.cfg` is already provided in this repository. Copy it.
   It will be automatically used by Buildroot during `make` to generate images for sdcard
3. **U-Boot configuration:**
   First tpye
   ```bash
   make uboot-menuconfig
   ```
   Waiting until see meny config => select Boot options:
   - Set enable boot arguments to Y
   - Change the its parameter from root=/dev/mtdblock3 to root=/dev/mmcblk0p2
   - Set enable a default value for bootcmd to be Y
   - Update the bootcmd value to: mmc dev 0; fatload mmc 0:1 0x80008000 boot.scr; source 0x80008000
   
   OK Save and Exit
5. **Build and Flash**
   Build:
   ```bash
   make
   ```
   OR faster
   ```bash
   make -j4
   ```
   Flash:
   ```bash
   sudo dd if=<Your img link> of=<Your sdcard link> bs=4M status=progress
   sync
   ```
6. **Check the results**
   
   Wiring between Lichee Pi Nano and USB-to-TTL:
   - U0TX (Chip) <-> RX (USB TTL)
   - U0RX (Chip) <-> TX (USB TTL)
   - GND <-> GND
   - 5V <-> 5V

   Open PuTTY with:
   - serial line: Your COM
   - Speed: 115200
     
   Result:
   <img width="1366" height="728" alt="image" src="https://github.com/user-attachments/assets/47d8ea91-5413-481a-8022-a967d375c871" />




    
