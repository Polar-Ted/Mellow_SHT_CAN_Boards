# HowTo for configuring the Mellow SHT-36 and SHT_42 CANBUS tool boards for Klipper. 



**This guide assumes you alreay have a working Klipper installation**

## Setup Steps

### Compile Toolboard Firmware
- ssh to your pi console
- CD to the klipper directory
```
cd klipper
```
- Run MAKE Clean
```
make clean
```
- open menuconfig
```
make menuconfig
```
- Set the following options      
![Menu Config](./images/makemenuconfig_screenshot.png)

- compile the firmware
```
make
```
- You should now have a klipper.bin file at ~/klipper/out/

### Burn Firmware to Tool Board

-Install the boot jumper to the Boot0 pin and 3.3v pin as pictured. 
![SHT burn jumper placement](https://mellow.klipper.cn/images/boards/fly_sht36_42/usbflash.png)

- Install the burning utility to your klipper host system
```
sudo apt install dfu-util -y
```
- Use USB-C data cable to connect the SHT board to the klipper host, Make sure that the jumper is installed before connecting the USB cable.      
- Run LSUSB to see if the connection is successful, copy the USB ID in the blue box. Note that the board is in DFU mode. 
```
lsusb
```
![USUSB Results](https://mellow.klipper.cn/images/boards/fly_sht36_42/6.png)

- Burn the firmware , replace 0483:df11 in the following command with the USB ID copied earlier
```
dfu-util -a 0 -d 0483:df11 --dfuse-address 0x08000000 -D ~/klipper/out/klipper.bin
```

-You should get a download progress bar and a success when the burning is complete. 
![burn complete](https://mellow.klipper.cn/images/boards/fly_sht36_42/7.png)

- If the burn was sucessful remove the USB cable and the jumper from the Boot0 pin
 
### Pi configuration

## Relevent links      
### Mellow tool board documentation      
https://mellow.klipper.cn/?spm=a2g0o.detail.1000023.17.566a6b5carjmy3#/board/fly_sht36_42/pins      

### Pin assignments      
https://mellow.klipper.cn/?spm=a2g0o.detail.1000023.17.566a6b5carjmy3#/board/fly_sht36_42/pins      

### Mellow Schematics        
https://github.com/Mellow-3D/Klipper-CAN-Toolboards      

### Klipper CANBUS Documentation     
https://www.klipper3d.org/CANBUS.html      

