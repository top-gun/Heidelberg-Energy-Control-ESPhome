The wallbox "Heidelberg Energy Control" is a robust BEV charger with an excellent price. It can be installed with load management or be used as an endpoint for charge-on-solar where you restrict charging speed to use only solar-energy to charge the vehicle.

![alt text](https://github.com/top-gun/Heidelberg-Energy-Control-ESPhome/blob/main/pictures/Heidelberg.JPEG "Wallbox")

For charge-on-solar, the easiest solution is to connect the box with a house-automation system like Home Assistant.

![alt text](https://github.com/top-gun/Heidelberg-Energy-Control-ESPhome/blob/main/pictures/Heidelberg-open.JPEG "Wallbox")

# Hardware requirements:

An ESP32 - they are in the range of 5-10 Euro. I picked one with 230V-input, about 8 Euro on Ali Express: https://de.aliexpress.com/item/1005003721730927.html

Of course, you may chose a standard ESP32-Devkit-PCB, they are cheaper but need a 5V supply.

With the 230V-version, you need an extra FTDI-adapter because the 230V-version doesn't come with a USB-port. They are pretty generic. I picked this one because I wanted one with a USB-C connector. Many come with the outdated Mini-USB connector. https://de.aliexpress.com/item/1005006150051861.html

A RS485-adapter for 2 Euro from Ali Express - they are about 10 Euro for a pack of 2 from Amazon, or 0.99 Euro on AliExpress: https://de.aliexpress.com/item/1005007176292527.html

# Hardware preparation:

<ins> ESP32:</ins>

On the ESP32, solder the 6-pin-connector to the PCB. The longer ones are not needed and should not be installed as we want the least open metal parts in the wallbox.

<ins> RS485 converter:</ins>

On the 4-pin-cable that comes with the RS485 adapter, you need to add a 4-pin Dupont connector to connect it to the ESP32. The most convenient order is to use 1:1 the same color order on the Dupont as on the JST-XH connector that goes into the RS485. See the picture for orientation.

![alt text](https://github.com/top-gun/Heidelberg-Energy-Control-ESPhome/blob/main/pictures/ESP32_and_RS485.JPEG "ESP32 wired with RS485")

# Software installation:

For this step, disconnect the ESP32 from the RS485 and connect it to the FTDI programmer. 230V is NOT needed, the ESP32 gets current from the FTDI programmer. The FTDI's USB goes into your Raspberry Pi, or whatever computer you use to run Home Assistant. 

In ESPHome builder add-on, import the yaml file into a new project, and select Install, then "Plug into the computer running ESPhome device builder". After the usual 5-10 minutes, the ESP32 gets programmed. If it doesn't connect right away, you may need to press both buttons on the ESP32 together.

Wiring the ESP32 and RS485:

It gets connected to the wallbox with a two-wire connection called Modbus, it's electrically also known as RS-485. The adapters are cheap and easy to install. They connect with four wires to the ESP32: Ground, 5V (3.3V would be ok too), RX and TX. RX and TX transfer the data to and from the adapter. The ESP32 has so-called "General Purpose Input/Output" pins that are usable for this. I chose the pins GPIO01 and GPIO03 for my ESP32. Depending on the model, these might be used for the USB adapter. It is also common to use GPIO 16 and GPIO 17, or GPIO 21 and GPIO 22. Just chose them based on the schematics that you get with your ESP32 and update the config file, they go into lines 84-85:


```
# UART configuration for RS485
uart:
  id: mod_uart
  tx_pin: GPIO01 # TX pin for RS485 module
  rx_pin: GPIO03 # RX pin for RS485 module
```
