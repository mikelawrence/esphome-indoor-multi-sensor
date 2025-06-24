# About

You need to make some choices before programming the ESPHome Indoor Multi-Sensor. 

First is the Sensor package. You can either choose Package A, the Sensirion SEN6X sensor package which is an all-in-one sensor package. Or you can choose Package B which is a lower cost discrete sensor set that is missing particulate matter sensors. 

Next up is what type of radar sensor you plan on including. There are 4 choices here, Hi Link LD2410 (My favorite), DFRobot C4001 (SEN0609 and SEN0610), Hi-Link LD2450 Multi-Target Multi-Zone (MTMZ) and the Hi-Link LD2450 Single-Target Single-Zone (STSZ). 

Each of these these builds include all supported sensors: pressure, light, sound level, power/energy monitor and speaker. If there are sensors you wish to not include, take control of the sensor and start configuring.

If you are really new to ESPHome I would recommend searching on Youtube for getting started videos.

## Installation

Use the buttons below to install pre-built firmware directly to your device via USB.

| Sensor Pkg | Radar LD2410 | Radar C4001 | Radar LD2450-MTMZ | Radar LD2450-STSZ |
|---|---|---|---|---|
| Pkg A | <esp-web-install-button manifest="firmware/sensor-pkg-a-ld2410.manifest.json"></esp-web-install-button> | <esp-web-install-button manifest="firmware/sensor-pkg-a-c4001.manifest.json"></esp-web-install-button> | <esp-web-install-button manifest="firmware/sensor-pkg-a-ld2450-mtmz.manifest.json"></esp-web-install-button> | <esp-web-install-button manifest="firmware/sensor-pkg-a-ld2450-stsz.manifest.json"></esp-web-install-button> |
| Pkg B | <esp-web-install-button manifest="firmware/sensor-pkg-b-ld2410.manifest.json"></esp-web-install-button> | <esp-web-install-button manifest="firmware/sensor-pkg-b-c4001.manifest.json"></esp-web-install-button> | <esp-web-install-button manifest="firmware/sensor-pkg-b-ld2450-mtmz.manifest.json"></esp-web-install-button> | <esp-web-install-button manifest="firmware/sensor-pkg-b-ld2450-stsz.manifest.json"></esp-web-install-button> |

<script type="module" src="https://unpkg.com/esp-web-tools@10/dist/web/install-button.js?module"></script> 

## Configuration

When you take control there are two sections you need to configure. In the ```substitutions:``` section you will find settings for many of the available sensors.

```yaml
substitutions:
  name: "sensor-pkg-a-c4001"
  friendly_name: "Indoor Multi-Sensor Pkg A Radar C4001"

  # Settings
  use_pir_in_presence: "true"
  # Status LED Configuration
  status_day_brightness: "0.5"
  status_night_brightness: "0.20"
  status_daytime_lux: "12.0"
  status_nighttime_lux: "7.0"
  # CO Configuration
  co_temp_id: "temperature"
  co_offset: "0.0"
  co_sensitivity: "2.000e-9"
  co_manufacture_date: "Unknown"
  co_serial_number: "111111111111111111"
  # SEN6X Configuration
  sen6x_temperature_offset: "0.0"
  sen6x_humidity_offset: "0.0"
  sen6x_output_rate: "60s"
  sen6x_co2_calibration_date: "Unknown"
  # INA2XX Configuration
  INA2XX_output_rate: "60s"
  # Automatically controlled Vent
  vent_ha_entity: fan.vent_id
  vent_use_humidity: "true"
  vent_use_co2: "true"
  vent_min_on_time: "15"
  vent_hum_on_trigger: "10.0"
  vent_hum_off_trigger: "2.5"
  vent_co2_on_trigger: "1100"
  vent_co2_off_trigger: "900"
```

The ```packages:``` section will allow you to disable sensors by commenting out it's included package. Note: do not comment out any Main Config Files.

```yaml
packages:
  remote_package_files:
    url: https://github.com/mikelawrence/ESPHome-Indoor-Multi-Sensor
    refresh: 1h
    files:
      # Main Config Files
      - config/common/pkga-sen6x.yaml
      - config/common/radar-c4001.yaml

      # ADDITIONAL SENSORS
      # Carbon Dioxide (CO) - Figaro TGS5141 sensor
      - config/common/add-co.yaml
      # Energy Usage - INA2XX sensor to measure Voltage, Power and Energy Usage
      - config/common/add-ina2xx.yaml
      # Light Level - TSL2591 sensor
      - config/common/add-tsl2591.yaml
      # Barometric Pressure - BMP581 sensor
      - config/common/add-bmp581.yaml
      # Microphone - Adds Sound Level meter
      - config/common/add-microphone.yaml

      # FEATURES
      # SEN6X Pressure Compensation - Adds pressure compensation to SEN6X
      - config/common/feat-sen6x-press-comp.yaml
      # Automatic Vent - Use humidity and CO₂ control a bathroom vent
      # - config/common/feat-auto-vent.yaml
```
### Settings

```yaml
  # Settings
  use_pir_in_presence: "true"
  # Status LED Configuration
  status_day_brightness: "0.5"
  status_night_brightness: "0.20"
  status_daytime_lux: "12.0"
  status_nighttime_lux: "7.0"
```

These are base sensor settings available for any configuration.

+ **use_pir_in_presence** (*Optional*, bool): 
+ **status_day_brightness** (*Optional*, bool): 
+ **status_night_brightness** (*Optional*, bool): 
+ **status_daytime_lux** (*Optional*, bool): 
+ **status_nighttime_lux** (*Optional*, bool): 

### Carbon Monoxide(CO) Configuration

```yaml
      # Carbon Dioxide (CO) - Figaro TGS5141 sensor
      - config/common/add-co.yaml
```

If you have included this package the CO sensor is enabled and there are substitutions you need to edit.

```yaml
  # CO Configuration
  co_temp_id: "temperature"
  co_offset: "0.0"
  co_sensitivity: "2.000e-9"
  co_manufacture_date: "Unknown"
  co_serial_number: "111111111111111111"
```

+ **co_temp_id** (*Optional*, ID): The accuracy of the CO sensor is improved with temperature compensation. This is the id of the temperature sensor to use.
+ **co_offset** (*Optional*, float): The CO sensor and circuit will have a offset. Use this to subtract out this eeor.
+ **co_sensitivity** (*Optional*, float): The CO has a default sensitivity ot 2.000e-9A per CO ppm. There is a QR code on the sensor that reports the factory measured sensitivity
+ **co_manufacture_date** (*Optional*, float): From the QR code on the sensor. The sensor has a 10 year life expectancy.
+ **co_serial_number** (*Optional*, float): From the QR code on the sensor. Not necessary but might come in handy down the road.

***
