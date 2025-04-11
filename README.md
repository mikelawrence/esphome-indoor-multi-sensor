# ESPHome Indoor Multi-Sensor

There are a lot of presence sensors using a combination of PIR and High Frequency Radar Human Presence Detectors to get a combination of quick response and low movement detection like when sleeping. Good examples of these types of sensor are the [Everything Presence One](https://shop.everythingsmart.io/products/everything-presence-one-kit) and [Roomsense IQ](https://www.roomsenselabs.com/). The later has a sensor module that you can add to support all sorts readings like Particular Matter (PM) Carbon Monoxide. But since I like to do everything myself. I started thinking about how I would approach this and more specifically what sensors did I want.

I settled on the following characteristics:

* ESP32-S3
* ESPHome software development
* USB-C Power
* Multiple options for Radar presence detection modules
  - Hi-Link LD2410B
  - Hi-Link LD2450
  - DFRobot C4001 (SEN609 and SEN610)
* PIR (Panasonic)
* Sensirion SEN66 All-in-One
  - Temperature
  - Humidity
  - PM
  - CO2
  - VOC
  - NOX
* Or individual Sensors
  - Sensirion SHT4X Temperature and Humidity
  - Sensirion SCD4X CO2
  - Sensirion SGP4X VOC and NOX
* Bosch BMP581 Pressure
* Figaro TGS5141 Carbon Monoxide (CO)
* AMS TSL2591 Light
* Microphone for sound levels
* A Speaker for alerts

## Status
Rev - is being fabricated right now. Wait for updates before using the design.

## Design Decisions
### ESP32 and ESPHome
ESPHome is closely aligned with Home Assistant. In fact they are the same company. Home Assistant uses ESPHome for some of their hardware like the [Home Assistant Voice Preview Edition](https://www.home-assistant.io/voice-pe/) or [Bluetooth Proxy](https://esphome.io/components/bluetooth_proxy.html). I chose a newer ESP32-S3 for this design which also has Bluetooth support. 

### USB-C Power
I'm not using USB-C Power Delivery but I am expecting 5V @ 3A which is readily available if you have the right USB-C Power brick.

### Radar based Human Presence Sensors
Both the Hi-Link sensors (LS2410B and LD2450) are supported by ESPHome directly but the DFRobot C4001 is not. I wrote an ESPHome External Component available [here](https://github.com/mikelawrence/ESPHome-Components). Physically the board has the LD2410 and LD2450 sharing the same physical space but the DFRobot C4001 can exist next to either a LD2410 or LD2450. I added circuitry so that you can choose which sensor to operate (they are all 24 GHz and probably will not work very well when both are powered simultaneously). 

### Sensors

## Enclosure

## Operation
