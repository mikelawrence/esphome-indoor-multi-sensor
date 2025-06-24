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

When you take control there are two sections you need to configure. In the ```substitutions:``` section you will find settings for many of the available sensors. Note: All Substitutions are strings even if the represent numbers.

```yaml
substitutions:

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
  co_serial_number: "Unknown"
  # SEN6X Configuration
  sen6x_temperature_offset: "0.0"
  sen6x_humidity_offset: "0.0"
  sen6x_co2_calibration_date: "Unknown"
  # Automatically controlled Vent
  vent_ha_entity: "fan.vent_id"
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
      - config/common/feat-auto-vent.yaml
```

### Sensor Package A Configuration

```yaml
      # Main Config Files
      - config/common/pkga-sen6x.yaml
```

```yaml
  # SEN6X Configuration
  sen6x_temperature_offset: "0.0"
  sen6x_humidity_offset: "0.0"
  sen6x_co2_calibration_date: "Unknown"
```

Sensor Package B sensor are all in one package with one interface.

+ **sen6x_temperature_offset** (*Required*, float):  The SEN66 temperature sensor will have a offset. Use this to subtract out this error. Default is 0.0.
+ **sen6x_humidity_offset** (*Required*, float): The SEN66 humidity sensor will have a offset. Use this to subtract out this error. Default is 0.0.
+ **sen6x_co2_calibration_date** (*Required*, string): The date the when the SEN66 CO₂ sensor was last calibrated. Periodic calibration is recommended.

### Sensor Package B Configuration

```yaml
      # Main Config Files
      - config/common/pkgb-sht4x.yaml
      - config/common/pkgb-scd4x.yaml
      - config/common/pkgb-sgp4x.yaml
```

```yaml
  # SHT4x Configuration
  sht4x_temperature_offset: "0.0"
  sht4x_humidity_offset: "0.0"
  # SCD4X Configuration
  scd4x_co2_calibration_date: "Unknown"
```
Sensor Package B is actually several sensor used together.

+ **sht4x_temperature_offset** (*Required*, float):  The SHT4X temperature sensor will have a offset. Use this to subtract out this error. Default is 0.0.
+ **sht4x_humidity_offset** (*Required*, float): The SHT4X humidity sensor will have a offset. Use this to subtract out this error. Default is 0.0.
+ **scd4x_co2_calibration_date** (*Required*, string): The date the when the SCD4X CO₂ sensor was last calibrated. Periodic calibration is recommended.

### Settings Substitutions

```yaml
substitutions:
  # Settings
  use_pir_in_presence: "true"
  # Status LED Configuration
  status_day_brightness: "0.5"
  status_night_brightness: "0.15"
  status_daytime_lux: "12.0"
  status_nighttime_lux: "7.0"
```

These are base sensor settings available for any configuration.

+ **use_pir_in_presence** (*Required*, bool): When ```true``` the PIR sensor is or'ed with the mmWave presence sensor. Default is ```true```.
+ **status_day_brightness** (*Required*, float): The brightness of the Status LED when the room is bright enough to be considered daytime. A number from 0.0 - 1.0, where 1.0 is full brightness. Default is 0.5.
+ **status_night_brightness** (*Required*, float): The brightness of the Status LED during daytime hours. A number from 0.0 - 1.0, where 1.0 is full brightness. Default is 0.15.
+ **status_daytime_lux** (*Required*, float):  The light level in lux where Status LED switches to nighttime. Default is "12.0"
+ **status_nighttime_lux** (*Required*, float): The light level in lux where Status LED switches to daytime.

### Carbon Monoxide(CO) Configuration

```yaml
      # Carbon Dioxide (CO) - Figaro TGS5141 sensor
      - config/common/add-co.yaml
```

If you have included this package the CO sensor is enabled and there are substitutions you need to edit.

```yaml
substitutions:
  # CO Configuration
  co_temp_id: "temperature"
  co_offset: "0.0"
  co_sensitivity: "2.000e-9"
  co_manufacture_date: "Unknown"
  co_serial_number: "111111111111111111"
```

+ **co_temp_id** (*Required*, ID): The accuracy of the CO sensor is improved with temperature compensation. This is the id of the temperature sensor to use.
+ **co_offset** (*Required*, float): The CO sensor and circuit will have a offset. Use this to subtract out this sensor. Default is 0.0.
+ **co_sensitivity** (*Required*, float): The CO has a default sensitivity ot 2.000e-9A per CO ppm. There is a QR code on the sensor that reports the factory measured sensitivity.
+ **co_manufacture_date** (*Required*, string): Sensor manufacture date from the QR code on the sensor. The sensor has a 10 year life expectancy.
+ **co_serial_number** (*Required*, string): Sensor serial number from the QR code on the sensor. Not necessary but might come in handy down the road.

You can keep the default sensitivity but it would be better to scan the barcode and use the factory measured sensitivity. The [datasheet](https://github.com/mikelawrence/ESPHome-Indoor-Multi-Sensor/blob/main/pcb/datasheets/TGS5141%20CO%20Sensor/tgs5xxx_application%20note(en)_rev01.pdf) has information on the barcode and how to parse.

### Carbon Monoxide(CO) Configuration

```yaml
      # Automatic Vent - Use humidity and CO₂ control a bathroom vent
      - config/common/feat-auto-vent.yaml
```

```yaml
  # Automatically controlled Vent
  vent_ha_entity: "fan.vent_id"
  vent_use_humidity: "true"
  vent_use_co2: "true"
  vent_min_on_time: "15"
  vent_hum_on_trigger: "10.0"
  vent_hum_off_trigger: "2.5"
  vent_co2_on_trigger: "1100"
  vent_co2_off_trigger: "900"
```

Humidity and CO₂ can automatically control a bathroom vent.

+ **vent_ha_entity** (*Required*, ID): This is the Home Assistant fan or switch ```id``` that controls the bathroom vent.
+ **vent_use_humidity** (*Required*, bool): When ```true``` humidity is used to automatically turn the vent on/off. Default is ```true```.
+ **vent_use_co2** (*Required*, bool): When ```true``` CO₂ is used to automatically turn the vent on/off. Default is ```true```.
+ **vent_min_on_time** (*Required*, float): When the vent was turned on automatically by high humidity or CO₂ this is how long the vent will stay on after humidity or CO₂ is back to normal. Default is 15.
+ **vent_hum_on_trigger** (*Required*, ID): Humidity change from baseline that will turn the vent on. Must be a positive number. Default is 10.0%.
+ **vent_hum_off_trigger** (*Required*, ID): Humidity difference from baseline that will indicate the end of high humidity. In general this is less than "vent_hum_on_trigger". Sometimes humidity from a shower just doesn't want to get back to where it was. If you make this number zero or less you might cause the vent to stay on. Default is 2.5%.
+ **vent_co2_on_trigger** (*Required*, ID): The level of CO₂ that will automatically turn the vent on. Default is 1100.
+ **vent_co2_off_trigger** (*Required*, ID): The level of CO₂ that will automatically turn the vent off. Should be 100-200 points less than the "vent_co2_on_trigger". Default is 900 ppm.
