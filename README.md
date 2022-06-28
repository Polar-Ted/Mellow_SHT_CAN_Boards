# HowTo for configuring the Mellow SHT-36 and SHT_42 CANBUS tool boards for Klipper. 

## Relevent links      
### Mellow tool board documentation      
https://mellow.klipper.cn/?spm=a2g0o.detail.1000023.17.566a6b5carjmy3#/board/fly_sht36_42/pins      

### Pin assignments      
https://mellow.klipper.cn/?spm=a2g0o.detail.1000023.17.566a6b5carjmy3#/board/fly_sht36_42/pins      

### Mellow Schematics        
https://github.com/Mellow-3D/Klipper-CAN-Toolboards      

### Klipper CANBUS Documentation     
https://www.klipper3d.org/CANBUS.html      

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

### Burn Firmware to Tool Board

### Pi configuration

