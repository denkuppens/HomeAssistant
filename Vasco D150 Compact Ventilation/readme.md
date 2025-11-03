# What is this?

**This is an EspHome based modbus controller to integrate the Vasco D150 ventilation unit into Home Assistant.**

![Screenshot of the controller in Home Assistant](./HASS_EspHomeVascoD150.png)

# How does it work?

It uses an ESP32 micro controller and a TTL To RS485 Module which is connected to the VASCO D150 controller. The ESP32 runs an EspHome program that converts the MODBUS registers to 
sensors and controls in Home Assistant as shown in the screenshot above.

# Installation

EspHome, Home Assistant should be installed and operational first. 
Program an ESP32 with the YAML file in this repository, connect the controller to the RS485 adapter and that one to the VASCO modubus connector. 

![Used RS485 adapter](./rs485module.png){width=50%}

![VASCO PCB](./VascoPCB.jpg){width=50%}


# Special feature

You can turn the unit off via Home Assistant which is usually not allowed. However it is NOT advised to turn the unit off. USE AT YOUR OWN RISK!

# D150 Modbus documentation

![Modbus datasheet](./90.01.06.53-Handleiding-Modbus-registers-SAB-RVU-1.pdf)