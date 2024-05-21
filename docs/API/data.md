# Sensor Data

Each sensor may report multiple data of different types e.g. temperature, humidity, battery level and sensor status. These data types (called `var_name` in the API) can be obtained independently for each sensor (including [historic](https://apiv2.lightfi.io/docs#/default/read_sensor_historic_data_sensors__sensor_id___var_name__history_get) and [daily metric](https://apiv2.lightfi.io/docs#/default/read_sensor_daily_data_range_sensors__sensor_id___var_name__daily_get) data) or all the latest values can be listed for a [sensor](https://apiv2.lightfi.io/docs#/default/read_latest_sensor_information_sensors__sensor_id__get) or [location](https://apiv2.lightfi.io/docs#/default/read_location_direct_children_locations__location_id___get) (including all sensors in a location), the latter route being very useful for e.g. the construction of live dashboards indicating all sensor data on a floor.

## Available data / `var_name` types

| `var_name` | Example Value | Unit | Description |
|-----------------|---------------|------|-----------------|
| `BACnetReads` | 10      | count        | number of BMS queries of this sensor in the last period |
| `batteryLevelPct` | 90  | %       | approximate percentage of battery remaining for battery powered sensors |
| `CO2ppm` | 850  | ppm | measured Carbon Dioxide concentration in parts per million |
| `clientsBLE` | 20  | count | number of occupancy indicating devices observed on BLE |
| `clientsWiFi` | 20  | count | number of occupancy indicating devices observed on WiFi |
| `energyInkWh` | 2000.0  | kWh | Total energy consumed (directional rather than net) e.g. electricity meter reading |
| `illuminanceArb` | 20.0  | arbitrary | measured light level |
| `motionEvent` | 1  | arbitrary | Each data point indicates a detected motion event, the value can be ignored but gives an indication of the number of recent events (see [below](#motionevent-data)) |
| `occSignatures` | 2  | count | measured signatures (people) under the sensor |
| `onlineStatus` | 1  | enum | Indicates online status of the sensor (1 = online, 0 = offline, -1 = rebooting, -2 = retired) |
| `particulateMatter` | 10  | µg/m³ | measured particulate matter level (PM2.5 - PM1/PM10 may be available as extra data) |
| `pressuremBar` | 1000  | mBar | measured air pressure |
| `relativeHumidity` | 50.0  | % | measured relative humidity of air |
| `temperatureC` | 20.1  | ℃ | measured temperature |
| `VOCppb` | 1000  | ppb | measured Volatile Organic Compounds concentration in parts per billion |

## `motionEvent` data

`motionEvent` data (as returned by PIR motion sensors) is slightly different from most other data in that the main significance is the timestamp of the recorded data, with the given value not so significant.
The raw live and historic `motionEvent` data should be thought of more as an event list: every data item, indicates a detected motion event.
Note: `motionEvent`s are throttled by default to only record one event per 10 seconds, so after the first event the sensor itself may detect several instances of motion in the next 10 seconds but you will only receive the next `motionEvent` for an event at least 10 seconds after the first one.

Returning data in this way can be an advantage as it allows the user to create applications where a custom timeout threshold for "Occupied"/"Unoccupied" can be specified.
For example, on [LightFi portal](https://portal.lightfi.io) you can use the sliders on the dashboard page in the "Motion Events" data dropdown to select e.g. "Occupied" if motion within the last 10 minutes and "Available" if no motion within the last 2 hours, allowing a live dashboard that can be suited to the customer preference.
Note: The complication with returning data in this way that if the sensor is "offline" (onlineStatus not equal to 1) then you need to know this too in order to distinguish between "no motionEvent" and "missing sensor".

#### `motionEvent` value
The numeric value returned when querying raw live/historic `motionEvent` data is generally not needed but does give an indication of the number of recent motion events.
The value is derived from a running count of motion events recorded by the sensor scaled using the following formula to reset/reduce the count after a period of inactivity:

```python
  value = last_value * min(1, 600 / (max(timestamp - last_time, 1))) + 1
```
(`timestamp` and `last_time` in seconds, `last_value` and `last_time` will be 0 for first data, `value` is always rounded to the nearest integer)

Thus if the count will always increase by 1 for every event seen less than 10 minutes (600 seconds) after the previous but will decrease for events seen after longer time gaps (likely resetting to 1 if the gap is significantly longer than 10 minutes).
This allows a quick view of the occupancy level of a space, distinguishing between sensors that have been observing longer periods of occupancy and those with only infrequent events.

### Utilisation metric data (`/daily` routes)

In order to calculate useful utilisation data for the metric routes available e.g. [for a sensor](https://apiv2.lightfi.io/docs#/default/read_sensor_daily_data_range_sensors__sensor_id___var_name__daily_get) or [for a whole floor](https://apiv2.lightfi.io/docs#/default/read_location_direct_child_daily_data_range_locations__location_id___var_name__daily_get) a fixed threshold timeout for occupancy indication has been used.
Utilisation (`utl`) percentage numbers returned by these routes are calculated based on a 10 minute threshold i.e. if there is less than 10 minutes between motion events the desk will be counted as in use, if more than 10 minutes as not in use.

These routes allow you to restrict the hours and days for which the overall data is calculated, as is possible for other data/`var_name`, unlike other data the values are returned as `utl` (percentage utilisation) rather than `min`/`avg`/`max` values during the time period.

If you desire to implement your own utilisation metric calculations with a different "no presence" timeout, you are still able to do this from the raw historic motionEvents data. (Though doing so will obviously be slower than using the pre-calculated values.)

Note: `utl` values are reported as a decimal between 0 and 1 indicating the percentage occupancy in that hour e.g. 0.79 is 79% utilised. Since this numbers are calculated for the timezone of the sensor, in regions with daylight savings time it is possible to get a `utl` value up to a maximum of 2 for the hour when the clocks go back, since this hour will occur 'twice' during that night.
