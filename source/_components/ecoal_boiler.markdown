---
layout: page
title: "eCoal water boiler controller"
description: "Instructions on how to integrate eSterownik.pl eCoal.pl controller into Home Assistant."
date: 2018-11-29 10:00
sidebar: true
comments: false
sharing: true
footer: true
# logo: raspberry-pi.png
ha_category: Water heater
ha_release: 0.87
ha_iot_class: Local Polling
redirect_from:
 - /components/sensor.ecoal_boiler/
 - /components/switch.ecoal_boiler/
---

The `ecoal_boiler` component is the base for pumps and sensors managed by [esterownik.pl eCoal boiler controller](https://esterownik.pl/nasze-produkty/ecoal).

## {% linkable_title Configuration %}

To use your eCoal pumps or sensors in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
ecoal_boiler:
  host: YOUR_IP_ADDRESS
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
  switches:
    monitored_conditions:
      - central_heating_pump
      - central_heating_pump2
  sensors:
    monitored_conditions:
      - outdoor_temp
      - indoor_temp
```

{% configuration %}
host:
  description: The IP address or the hostname of eCoal controller.
  required: true
  type: string
username:
  description: Login used to connect to controller.
  required: false
  default: admin
  type: string
password:
  description: Password for username.
  required: false
  default: admin
  type: string
switches:
  description: switch-related configuration options
  required: false
  type: map
  keys:
    monitored_conditions:
      description: controller available source of switches
      required: false
      type: list
      default: all (`central_heating_pump`, `central_heating_pump2`, `domestic_hot_water_pump`)
sensors:
  description: sensor-related configuration options
  required: false
  type: map
  keys:
    monitored_conditions:
      description: controller available source of sensors
      required: false
      type: list
      default: all (`outdoor_temp`, `indoor_temp`, `indoor2_temp`, `domestic_hot_water_temp`, `target_domestic_hot_water_temp`,
                    `feedwater_in_temp`, `feedwater_out_temp`, `target_feedwater_temp`,
                    `fuel_feeder_temp`, `exhaust_temp`)
{% endconfiguration %}
