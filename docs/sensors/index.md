# Sensor Overview

LightFi produce a range of sensors for gathering building data and facilitation of building control via integration with the Building Management System (BMS)

## LightFi Base

The LightFi "Base" sensor is a long-range, broad level occupancy sensor, which determines the percentage occupancy levels by measuring the number active devices on a floor area, with coverage of 250 – 1000 square meters depending on placement and number of walls etc. (the sensors can function through walls but the range is reduced).

The Base sensor also functions as a router/gateway for LightFi sensors, reporting data to the cloud and allowing configuration of sensor settings and sub-sensors via the portal or API interface.

If [BACnet](BACnet.md) enabled, this sensor will also function as a BACnet router device as a BACnet device, with it’s own BACnet network number (configurable via LightFi Portal). This will allow the Base sensor itself and all configured sub-sensors to be accessible via the BACnet/IP. 

## LightFi Sahara

The Sahara sensor measures: Carbon Dioxide (CO₂) levels with a dual-channel NDIR sensor and Particulate Matter (PM2.5).

The sensor requires a configuration with a LightFi Base Sensor in order to be visible on the BACnet network.

If BACnet enabled, this sensor will appear as a BACnet device on the BACnet network created by the BACnet router internal to the Base Sensor.

## LightFi Hoth

The Hoth sensor measures temperature, relative humidity and its own battery level.

The sensor requires a configuration with a LightFi Base sensor in order to be visible on the BACnet network.

If BACnet enabled, this sensor will appear as a BACnet device on the BACnet network created by the BACnet router internal to the Base sensor.

## LightFi X1

The X1 is a battery powered Passive Infrared (PIR) motion sensor, designed to detect binary desk or room occupancy. 

The sensor requires a configuration with a LightFi Base sensor in order to be visible on the BACnet network.

If BACnet enabled, this sensor will appear as a BACnet device on the BACnet network created by the BACnet router internal to the Base sensor.


## Install guide

See [Installation](./01_install.md)

For more information on BACnet objects for these devices, check [BACnet section](./BACnet.md).
