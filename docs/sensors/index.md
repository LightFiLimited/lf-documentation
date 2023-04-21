# LightFi Sensors

LightFi produce a range of sensors for gathering building data and facilitation of building control via integration with the Building Management System (BMS)

## LightFi Base

The LightFi "Base" sensor is a long-range, broad level occupancy sensor, which determines the percentage occupancy levels by measuring the number active devices on a floor area c.3,000 – 5,000+ sqft wide.

The Base sensor also functions as a router/gateway for LightFi sensors, with it’s own BACnet network number (configurable via LightFi Portal).

If BACnet enabled, this sensor will appear as a BACnet device on the BACnet network created by the router internal to this Base Sensor.

## LightFi Sahara

The Sahara sensor measures: Carbon Dioxide (CO2) levels with a dual-channel NDIR sensor and Particulate Matter (PM2.5).

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

PDF install guide (v1.2) - [Download](https://nextcloud.lightfi.io/index.php/s/F7HNYyGYDXkd2P9/download/LightFi_system_install_guide_v1.2.pdf) 

For more information on BACnet objects for these devices, check [BACnet section](./BACnet.md).
