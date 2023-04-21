# elco-remocon-net-appdaemon

ELCO Remocon.net AppDaemon app to fetch data from the gas boiler system via the Elco Remocon-Net cloud service and push them to home-assistant entities.

[![hacs_badge](https://img.shields.io/badge/HACS-Default-41BDF5.svg?style=for-the-badge)](https://github.com/hacs/integration)

## Installation

### HACS configuration

Make sure that you [have the AppDaemon discovery and tracking](https://hacs.xyz/docs/categories/appdaemon_apps) enabled for HACS.

### AppDaemon's python packages pre-requisites

No additional python packages are required.

## Configuration

You need to provide your `gateway_id`, which you would find at https://www.remocon-net.remotethermo.com

For this, you also need to provide your `username` and `password` to access the Elco Remocon-Net cloud service.

### secrets.yaml

You will need the following in your secrets.yaml file

```yaml
username: <YOUR_EMAIL>
password: <YOUR_PASSWORD>
gateway_id: <YOUR_INSTALLATION_NUMBER>
bearer_token: <HA_LONG_LIVE_TOKEN>
```

The `bearer_token` refers to a long-lived Home Assistant token.

### apps.yaml

Define your app like the following:

<!-- MARKDOWN-AUTO-DOCS:START (CODE:src=./apps.example.yaml) -->
<!-- The below code snippet is automatically added from ./apps.example.yaml -->
```yaml
remocon:
  module: elco-remocon-net-appdaemon
  class: Remocon
  plugin: HASS
  base_url: https://www.remocon-net.remotethermo.com
  username: <your_remocon_username>
  password: <your_remocon_password>
  bearer_token: <HA_LONG_LIVE_TOKEN>
  gateway_id: <your_remocon_gateway_id>
  # refresh_rate: 60 # optional
  #ha_url:  # optional, in case hassplugin ha_url undefined
```
<!-- MARKDOWN-AUTO-DOCS:END -->

## Usage

POSTs the data to the following to HA sensors:

| sensor | Description | source | Detail |
|--- |---|---|---|
| sensor.elco_outside_temperature | Elco Outside Temperature | plantData.outsideTemp | unit_of_measurement: °C  |
| sensor.elco_domestic_hot_water_storage_temperature | Elco Domestic Hot Water Storage Temperature | plantData.dhwStorageTemp | unit_of_measurement: °C |
| sensor.elco_domestic_hot_water_temperature | Elco Domestic Hot Water Temperature | plantData.dhwComfortTemp.value | unit_of_measurement: °C |
| sensor.elco_comfort_room_temperature_setpoint | Elco Comfort room Temperature setpoint | zoneData.chComfortTemp.value | unit_of_measurement: °C |
| sensor.elco_reduced_room_temperature_setpoint | Elco Reduced room Temperature setpoint | zoneData.chReducedTemp.value | unit_of_measurement: °C |
| sensor.elco_room_temperature | Elco Room Temperature | zoneData.roomTemp | unit_of_measurement: °C |
| sensor.elco_desired_room_temperature | Elco Desired Room Temperature | zoneData.desiredRoomTemp | unit_of_measurement: °C |
| sensor.elco_room_operation_mode_heating | Elco Room Operation mode heating | zoneData.mode.value | Protection, Automatic, Reduction, Comfort, Unknown |
| binary_sensor.elco_domestic_hot_water_storage_mode | Elco Domestic Hot Water Storage Mode | plantData.dhwMode.value |  |
| binary_sensor.elco_domestic_hot_water_storage_temperature_error | Elco Domestic Hot Water Storage Temperature Error | plantData.dhwStorageTempError |  |
| binary_sensor.elco_outside_temperature_error | Elco Outside Temperature Error | plantData.outsideTempError |  |
| binary_sensor.elco_heatpump_on | Elco HeatPump On | plantData.heatPumpOn |  |
| binary_sensor.elco_domestic_hot_water_enabled | Elco Domestic Hot Water Enabled | plantData.dhwEnabled |  |
| binary_sensor.elco_room_heating_is_active | Elco Room Heating is Active | zoneData.isHeatingActive |  |
| binary_sensor.elco_room_heating_is_request | Elco Room Heating is Request | zoneData.heatOrCoolRequest |  |

## Limitations

This appdaemon's app only supports 1 gateway.
Readonly access, not set point, at the moment.

## License

[MIT](LICENSE)
