The wallbox "Heidelberg Energy Control" is a robust BEV charger with an excellent price. It can be installed with load management or be used as an endpoint for charge-on-solar where you restrict charging speed to use only solar-energy to charge the vehicle.

For charge-on-solar, the easiest solution is to connect the box with a house-automation system like Home Assistant.

The necessary hardware is cheap:

An ESP32 - they are in the range of 5-10 Euro. I picked one with 230V-input, about 8 Euro on Ali Express.
A RS485-adapter for 2 Euro from Ali Express - they are about 10 Euro for a pack of 2 from Amazon.

The ESP32 needs to be compiled with the Yaml-file in https://github.com/top-gun/Heidelberg-Energy-Control-ESPhome/blob/main/Heidelberg.yaml . It gets connected to the wallbox with a two-wire connection called Modbus, it's electrically also known as RS-485. The adapters are cheap and easy to install. They connect with four wires to the ESP32: Ground, 5V (3.3V would be ok too), RX and TX. RX and TX transfer the data to and from the adapter. The ESP32 has so-called "General Purpose Input/Output" pins that are usable for this. I chose the pins GPIO01 and GPIO03 for my ESP32. Depending on the model, these might be used for the USB adapter. It is also common to use GPIO 16 and GPIO 17, or GPIO 21 and GPIO 22. Just chose them based on the schematics that you get with your ESP32 and update the config file, they go into lines 84-85:

´´´
# UART configuration for RS485
uart:
  id: mod_uart
  tx_pin: GPIO01 # TX pin for RS485 module
  rx_pin: GPIO03 # RX pin for RS485 module
´´´
