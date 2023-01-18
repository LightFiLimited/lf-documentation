# LightFi Dashboard/Portal

The LightFi Dashboard can be found at [https://portal.lightfi.io](https://portal.lightfi.io)

You require a login to use the dashboard, user accounts will be created for your organisation with
your initial sensor purchase. Additional accounts can be created by the Admin user(s) of you organisation.

## General Concepts

### Choosing the location

Use the selector in the top left to choose your location.
Each location can have sub-locations, this is usually indicated by an "ALL" drop-down,
use the drop-down to select the sub-location you are particularly interested in.
Repeat the process if you want to keep going to more more specific locations.
A typical location structure may be something like:

- Organisation Name > Site > Building > Floor

### Data sources

You can add or remove data you are interested in using the "Sources" button.
For example if you are interested in viewing Occupancy but not Environmental
data, you can add WiFi Occupancy and Motion Events but remove Temperature.

List of possible data sources:

| Source Name | Example Value | Unit | Description |
|-----------------|---------------|------|-----------------|
| BACnet | 10      | count        | number of BMS queries of this sensor in the last period |
| Battery | 90  | %       | approximate percentage of battery remaining for battery powered sensors |
| Bluetooth Occupancy | 20  | count | number of occupancy indicating devices observed on BLE |
| CO₂ | 850  | ppm | Carbon Dioxide concentration (parts per million) |
| Humidity | 50.0  | % | relative humidity of the air |
| Light level | 20.0  | arbitrary | measured light level |
| Motion Events | 1  | arbitrary | Used to display data from PIR motion sensors.|
| Online Status | Online  | none | Indicates the status of the sensor |
| Pollutants | 10  | µg/m³ | particulate matter level (PM2.5) |
| Pressure | 1000  | mBar | measured air pressure |
| Temperature | 20.1  | ℃ | measured temperature |
| Thermal Signatures | 2 | count | Used for HAL sensors to indicate number/position of detected people in field of view of the sensors|
| Volatile Organic Compounds | 1000  | ppb | measured Volatile Organic Compounds concentration in parts per billion |
| WiFi Occupancy | 20  | count | number of occupancy indicating devices observed on WiFi |

### User Details

Click your user name (top right) to select user features.

#### Changing Password

You can change your password from the "User Profile" screen.

#### Setting Two Factor Authentication (2FA / MFA)

You can setup two factor authentication for your login on the "User Profile" screen.
You should use a TOTP application such as Google Authenticator or Password Manager for iPhone/Mac to set this up.

## Views

### Dashboard Screen

Shows detailed live data for the selected location (drop-downs include sub-locations, map only shows the current location).

### Compare Screen

Used to graph different data and compare them.
Data sources, sensors and time range you would like to compare data over.
Can also generate a report for the selected data and time range
by selecting "Show Report" button.

### Calendar View Screen

Used to view longer term metric data for a selected Data Source (click on the selector on the Map to change selected data).
Can be used to compare typical values for Weekdays i.e. averaging all Mondays together etc. or to compare days
in a calendar format.

### Summary Screen

Simplified live information about current conditions in a location. Useful for mobile phone view.

### Sensor Info

Information about different sensor types.

### Configuration Screen (Admin)

Used to configure sensors and locations.


