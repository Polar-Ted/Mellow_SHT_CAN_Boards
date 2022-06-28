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
- Set the following options for CANBUS connection     
![Menu Config CANBUS](./images/makemenuconfig_screenshot.png)

- Set the following for USB Connection
![Menu Config USB](./images/makemenuconfig_screenshot_USB.png)
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

-You should get a download progress bar and File downloaded successfuly when the burning is complete. 
![burn complete](https://mellow.klipper.cn/images/boards/fly_sht36_42/7.png)

- If the burn was sucessful remove the USB cable and the jumper from the Boot0 pin
- The tool board is now ready to install as a klipper MCU. 
**Repeat these steps if a klipper update requires flashing new firmware to the MCU. 

### Link to CANBUS HAT or USB adapter configuration document will go here

### Klipper Host configuration

-CANBUS configuration
 -Finding the canbus_uuid for new micro-controllers¶
  Each micro-controller on the CAN bus is assigned a unique id based on the factory chip identifier encoded into each micro-controller. To find each micro-controller     device id, make sure the hardware is powered and wired correctly, and then run:


```~/klippy-env/bin/python ~/klipper/scripts/canbus_query.py can0
```
  If uninitialized CAN devices are detected the above command will report lines like the following:


```
Found canbus_uuid=11aa22bb33cc
```
  Each device will have a unique identifier. In the above example, 11aa22bb33cc is the micro-controller's "canbus_uuid".

   Note that the canbus_query.py tool will only report uninitialized devices - if Klipper (or a similar tool) configures the device then it will no longer appear in      the list.

 -Configuring Klipper¶
  Update the Klipper mcu configuration to use the CAN bus to communicate with the device - for example:

```
[mcu sht36]
canbus_uuid: 11aa22bb33cc
```


## Relevent links      
### Mellow tool board documentation      
https://mellow.klipper.cn/?spm=a2g0o.detail.1000023.17.566a6b5carjmy3#/board/fly_sht36_42/pins      

### Pin assignments      
https://mellow.klipper.cn/?spm=a2g0o.detail.1000023.17.566a6b5carjmy3#/board/fly_sht36_42/pins      

### Mellow Schematics        
https://github.com/Mellow-3D/Klipper-CAN-Toolboards      

### Klipper CANBUS Documentation     
https://www.klipper3d.org/CANBUS.html      

