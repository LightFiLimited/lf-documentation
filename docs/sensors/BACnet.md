# BACnet

Our solution consists in the use of an array of sensors to monitor multiple types of physical spaces (such as offices, airports, universities, ...).

These sensors are able to measure different types of data:

- [Hoth](#lightfi-hoth) - Temperature and Humidity;
- [Sahara](#lightfi-sahara) - CO2 and Particulate Matter;
- [X1](#lightfi-x1) - Presence;
- [Base](#lightfi-base) - Broad Level Occupancy Sensor.

One of the main features of the [Base](#lightfi-base) sensor is the wireless technology capabilities it has which allows it to act as a gateway for receiving data from the other sensors and send it to our [platform](http://portal.lightfi.io) for data visualisation. This feature, along with our BACnet/IP implementation, enables the Base to create BACnet representations of the actual wireless sensors.

![](../img/sensors/bacnet_diagram.svg)

## LightFi Base

The LightFi "Base" sensor is a long-range, broad level occupancy sensor, which determines the percentage occupancy levels by measuring the number active devices on a floor area c.3,000 – 5,000+ sqft wide.

| Object Type   | Object ID | Object Name                     | Present Value                |
|---------------|-----------|---------------------------------|------------------------------|
| Device        | (1)       | LightFi_LF-AABBCCDDEEFF         | N/A                          |
| Analog Input  | AI1       | WiFi Occupancy Raw (number)     | Current value / Default = 0|
| Analog Input  | AI2       | WiFi Occupancy Percentage       | (AI 1) / (AV 1) * 100        |
| Analog Value  | AV1       | WiFi Occupancy Maxium Setpoint  | Default = 100                |

Notes:

(1) Object ID can be configured via LightFi Portal;

(2) The Object Name field will depend on the device’s MAC Address. This value should start with "LightFi_LF-".

## LightFi Sahara

The Sahara sensor measures: Carbon Dioxide (CO2) levels with a dual-channel NDIR sensor and Particulate Matter (PM2.5).


| Object Type   | Object ID | Object Name                     | Present Value                |
|---------------|-----------|---------------------------------|------------------------------|
| Device        | (1)       | LightFi_LF5-AABBCCDDEEFF (2)    | N/A                          |
| Analog Input  | AI1       | Carbon Dioxide level (ppm)      | Current Reading / -999 if not initialised |
| Analog Input  | AI2       | Particulate Matter PM2.5        | Current Reading / -1 if not initialised   |
| Analog Input  | AI3       | RSSI                            | Current Reading / -999 if not initialised |

Notes:

(1) Object ID can be configured via LightFi Portal;

(2) The Object Name field will depend on the device’s MAC Address. This value should start with "LightFi_LF5-".

## LightFi Hoth

The Hoth sensor measures temperature, relative humidity and its own battery level.

| Object Type   | Object ID | Object Name                          | Present Value                               |
|---------------|-----------|--------------------------------------|---------------------------------------------|
| Device        | (1)       | LightFi_MS1-AABBCCDDEEFF (2)         | N/A                                         |
| Analog Input  | AI1       | Temperature value (Celsius)          | Current Reading / -1 if not initialised     |
| Analog Input  | AI2       | Relative Humidity value (Percentage) | Current Reading / -1 if not initialised     |
| Analog Input  | AI3       | RSSI                                 | Current Reading / -999 if not initialised   |
| Analog Input  | AI4       | Battery Level Value (Percentage)     | Current Reading / -999 if not initialised   |

Notes:

(1) Object ID can be configured via LightFi Portal;

(2) The Object Name field will depend on the device’s MAC Address. This value should start with "LightFi_MS1-".

## LightFi X1

The X1 is a battery powered Passive Infrared (PIR) motion sensor, designed to detect binary desk or room occupancy.


| Object Type   | Object ID | Object Name                      | Present Value                |
|---------------|-----------|----------------------------------|------------------------------|
| Device        | (1)       | LightFi_AMP-AABBCCDDEEFF (2)     | N/A                          |
| Analog Input  | AI1       | Motion Sensor (Presence)         | Current Reading / -1 if not initialised   |
| Analog Input  | AI3       | RSSI                             | Current Reading / -999 if not initialised |
| Analog Input  | AI4       | Battery Level Value (Percentage) | Current Reading / -999 if not initialised |

Notes:

(1) Object ID can be configured via LightFi Portal;

(2) The Object Name field will depend on the device’s MAC Address. This value should start with "LightFi_AMP-". 

## Enabling Sensors in BACnet Network

### Base Sensor

Activation of the Base Sensor can be made through the LightFi Portal by accessing "BACnet Config" in the desired base sensor configuration page and pressing the "Enable" button.

When activating the Base Sensor in our platform, the following fields are able to be configured:

- Building Network Number;
- VLAN Network Number;
- BACnet ID of the device.

After entering these fields, the device will take around 2 minutes to apply the changes.

### Wireless Sensors

All the other available wireless sensors can be activated in the same way, by accessing  "BACnet Config" in the desired wireless sensor configuration page and pressing the "Enable" button.

This will make a text field appear for entering its BACnet ID. Note that if a BACnet ID is not entered, our system will pick one and present it in the configuration page.

Contrary to the Base sensor, these will immediatly be available in the BACnet Network.
