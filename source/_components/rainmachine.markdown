---
layout: page
title: "RainMachine"
description: "Instructions on how to integrate RainMachine units within Home Assistant."
date: 2018-04-25 20:32
sidebar: true
comments: false
sharing: true
footer: true
logo: rainmachine.png
ha_category:
  - Irrigation
  - Binary Sensor
  - Sensor
  - Switch
ha_release: 0.69
ha_iot_class: Local Polling
redirect_from:
  - /components/binary_sensor.rainmachine/
  - /components/sensor.rainmachine/
  - /components/switch.rainmachine/
---

The `rainmachine` component is the main component to integrate all platforms related to [RainMachine smart Wi-Fi sprinkler controllers](http://www.rainmachine.com/).

There is currently support for the following device types within Home Assistant:

- Binary Sensor
- Sensor
- [Switch](#switch)

## {% linkable_title Base Configuration %}

To connect to your RainMachine device, add the following to your `configuration.yaml` file:

```yaml
rainmachine:
  controllers:
    - ip_address: 192.168.1.100
      password: YOUR_PASSWORD
```

To configure additional functionality, add configuration options beneath a `binary_sensor`, `sensor`, and/or `switches` key within the `rainmachine` sections of `configuration.yaml` as below:

```yaml
rainmachine:
  controllers:
    - ip_address: 192.168.1.100
      password: YOUR_PASSWORD
      binary_sensors:
        # binary sensor configuration options...
      sensors:
        # sensor configuration options...
      switches:
        # switch configuration options...
```

{% configuration %}
ip_address:
  description: The IP address or hostname of your RainMachine unit.
  required: optional
  type: string
password:
  description: Your RainMachine password.
  required: true
  type: string
port:
  description: The TCP port used by your unit for the REST API.
  required: false
  type: integer
  default: 8080
ssl:
  description: Whether communication with the local device should occur over HTTPS.
  required: false
  type: boolean
  default: true
scan_interval:
  description: The frequency (in seconds) between data updates.
  required: false
  type: integer
  default: 60
binary_sensors:
  description: Binary sensor-related configuration options.
  required: false
  type: map
  keys:
    monitored_conditions:
      description: The conditions to create sensors from.
      required: false
      type: list
      default: all (`extra_water_on_hot_days`, `freeze`, `freeze_protection`, `hourly`, `month`, `raindelay`, `rainsensor`, `weekday`)
sensors:
  description: Sensor-related configuration options.
  required: false
  type: map
  keys:
    monitored_conditions:
      description: The conditions to create sensors from.
      required: false
      type: list
      default: all (`freeze_protect_temp`)
switches:
  description: Switch-related configuration options.
  required: false
  type: map
  keys:
    zone_run_time:
      description: The default number of seconds that a zone should run when turned on.
      required: false
      type: integer
      default: 600
{% endconfiguration %}

## {% linkable_title Services %}

### {% linkable_title `rainmachine.disable_program` %}

Disable a RainMachine program. This will mark the program switch as
`Unavailable` in the UI.

| Service Data Attribute    | Optional | Description             |
|---------------------------|----------|-------------------------|
| `program_id`              |      no  | The program to disable  |

### {% linkable_title `rainmachine.disable_zone` %}

Disable a RainMachine zone. This will mark the zone switch as
`Unavailable` in the UI.

| Service Data Attribute    | Optional | Description             |
|---------------------------|----------|-------------------------|
| `zone_id`                 |      no  | The zone to disable     |

### {% linkable_title `rainmachine.enable_program` %}

Enable a RainMachine program.

| Service Data Attribute    | Optional | Description             |
|---------------------------|----------|-------------------------|
| `program_id`              |      no  | The program to enable   |

### {% linkable_title `rainmachine.enable_zone` %}

Enable a RainMachine zone.

| Service Data Attribute    | Optional | Description             |
|---------------------------|----------|-------------------------|
| `zone_id`                 |      no  | The zone to enable      |

### {% linkable_title `rainmachine.pause_watering` %}

Pause all watering activities for a number of seconds.

| Service Data Attribute    | Optional | Description                    |
|---------------------------|----------|--------------------------------|
| `seconds`                 |      no  | The number of seconds to pause |

### {% linkable_title `rainmachine.start_program` %}

Start a RainMachine program.

| Service Data Attribute    | Optional | Description          |
|---------------------------|----------|----------------------|
| `program_id`              |      no  | The program to start |

### {% linkable_title `rainmachine.start_zone` %}

Start a RainMachine zone for a set number of seconds.

| Service Data Attribute    | Optional | Description                                          |
|---------------------------|----------|------------------------------------------------------|
| `zone_id`                 |      no  | The zone to start                                    |
| `zone_run_time`           |      yes | The number of seconds to run; defaults to 60 seconds |

### {% linkable_title `rainmachine.stop_all` %}

Stop all watering activities.

### {% linkable_title `rainmachine.stop_program` %}

Stop a RainMachine program.

| Service Data Attribute    | Optional | Description          |
|---------------------------|----------|----------------------|
| `program_id`              |      no  | The program to stop  |

### {% linkable_title `rainmachine.stop_zone` %}

Stop a RainMachine zone.

| Service Data Attribute    | Optional | Description          |
|---------------------------|----------|----------------------|
| `zone_id`                 |      no  | The zone to stop     |

### {% linkable_title `rainmachine.unpause_watering` %}

Unpause all watering activities.

## {% linkable_title Switch %}

The `rainmachine` switch platform allows you to control programs and zones within a [RainMachine smart Wi-Fi sprinkler controller](http://www.rainmachine.com/).

### {% linkable_title Controlling Your Device %}

After Home Assistant loads, new switches will be added for every enabled program and zone. These work as expected:

- Program On/Off: starts/stops a program
- Zone On/Off: starts/stops a zone (using the `zone_run_time` parameter to determine how long to run for)

Programs and zones are linked. While a program is running, you will see both the program and zone switches turned on; turning either one off will turn the other one off (just like in the web app).
