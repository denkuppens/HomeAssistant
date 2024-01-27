# What is this?

This is a solution to track WIFI devices in a home using one or more OpenWrt WIFI access points in combination with Home Assistant.

Note: It might not be useable in your situation because you need:
- One or more router and accesspoints flashed with OpenWrt firmware (openwrt.org)
- Home Assistant (https://www.home-assistant.io/)
- MQTT server that is connected to Home Assistant
- Optional: WIFI Fast roaming enabled on all OpenWrt APs
- Optional: IOT devices, MQTT server and Home Assistant running in a protected VLAN which can't access the router
- To know how to create scripts and automations in Home Assistant 

# Why did I create this?

I was looking for a way to determine if my family members are home. There are a few very nice solutions out there but as far as I could tell they need direct access to the network router. I don't have that because I have all IOT devices connected to a VLAN which can't access the OpenWrt settings and I did not want to open a port for security reasons. 
Then I found the excellent work of Simon Christmann and Alexander Nagy on github providing a solution to publish network events to a MQTT server. The router and APs can connect to the VLAN so this was the first part of the solution. The other part was to make the presence visible in Home Assistant and to create automations based on the data. This is done via the HA automations and scripting functionality.

# How does it work?

Each OpenWrt AP/router runs a program which publishes (dis)connects to a central MQTT server. In my case the MQTT server is running on the same computer as Home Assistant.
Next, Home Assistant has an automation that triggers when the MQTT message is received and calls the script that I provide here. The script analyses the MQTT message and the script updates the text of a specific input_text helper. 
We have an access point on every level of our house. This setup shows what mobile phones are connected to what AP and displays that in Home Assistant. This information is then used to see where a person most likely is and to start the robot vacuum  on a particular floor if we are all on another floor. 
The reaction time of the detection is remarkably fast. Perhaps this is because I have setup the AP's in 'Fast roaming' mode. There is an excellent video tutorial how to do this: https://www.youtube.com/watch?v=kMgs2XFClaM

# Installation

Install presence_report on each OpenWrt AP/router in your home following this GitHub repository:
https://github.com/enesbcs/owrtwifi2mqtt
This is the most work if you have many APs.

In Home Assistant create a new script and copy-paste the contents of Script_To_what_wifi_access_point_is_a_device_connected.yaml to the script in 'Edit in yaml' mode. This can be done by clicking the three dots in the upper right corner of Home Assistant.
Name the script "To what wifi access point is a device connected?". 
Save the script.

Create 'input text' helpers for every device/person you want to track in the house. Do this in Home Assistant via Settings->Devices->Helpers->Create Helper. Select 'Text' in the dialog that pops up. 
Name the helper like 'Where is xxxxx in the house?'. Replace 'xxxxx' with the name of the person.

Find the MAC addresses of the devices you want to track. I got them via the OpenWrt router main interface: http://192.168.1.1

Note: convert the MAC addresses to lower case and change the ':' sign to '-'. So AA:BB:CC:DD:EE:FF becomes aa-bb-cc-dd-ee-ff.

Copy-paste the contents of Automation_Show_where_devices_are_in_the_house.yaml to the automation in 'Edit in yaml' mode. 
Name the automation 'Show to what wifi access pionts devices are connected'
Then change the automation to your MAC addresses and input_text helpers. 
### The order of MAC addresses need to match the order of the input_text helpers!!!!

# Screenshots

- Screenshot of the automation:
![Screenshot of the automation] (./Automation_screenshot.jpg?raw=true)

- Screenshot of the input_text entities:
![Screenshot of the input_text entities](./HomeAssistant_screenshot.jpg?raw=true)

- Screenshot of the input_text history:
![Screenshot of the input_text history entities] (./HomeAssistant_history_screenshot.jpg?raw=true)


# Credits

OpenWrt script from Alexander Nagy (https://github.com/enesbcs/owrtwifi2mqtt)

## Disclaimer

This small project was my fist time using YAML as 'programming language' beyond what I did with EspHome. It was horrible because creating the script was a frustrating experience. I'm sure my script can be improved in many ways but I'm just happy it works. I hope someone else finds it useful but don't expect me to provide a lot of support.