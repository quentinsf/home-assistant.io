---
layout: page
title: "Danfoss Air"
description: "How to integrate Danfoss Air HRV in Home Assistant."
date: 2019-01-14 20:00
sidebar: true
comments: false
sharing: true
footer: true
ha_category:
  - Climate
  - Binary Sensor
  - Sensor
  - Switch
ha_release: 0.87
logo: danfoss_air.png
ha_iot_class: Local Polling
redirect_from:
  - /components/binary_sensor.danfoss_air/
  - /components/sensor.danfoss_air/
  - /components/switch.danfoss_air/
---

The `danfoss_air` component allows you to access information from your Danfoss Air HRV unit.

*Note*: Danfoss Air CCM only accepts one TCP connection at a time. Due to this the component will not work while you have the HRV PC-Tool open.

There is currently support for the following device types within Home Assistant:

- [Binary sensor](#binary-sensor)
- [Sensor](#sensor)
- [Switch](#switch)

```yaml
# Example configuration.yaml entry
danfoss_air:
  host: IP_ADDRESS_OF_CCM
```

{% configuration %}
host:
  description: Danfoss Air CCM IP.
  required: true
  type: string
{% endconfiguration %}

## {% linkable_title Binary sensor %}

The following binary sensor is supported.

- **Bypass active:** Indicator if heat recovery is currrently bypassed.

## {% linkable_title Sensor %}

The following sensors are supported.

- **Outdoor temperature:** Outdoor air temperature.
- **Supply temperature:** Air temperature of the air supplied to the house.
- **Extract temperature:** Air temperature of the air extracted from the house.
- **Exhaust temperature:** Exhausted air temperature.
- **Remaining filter lifetime:** Reamining filter lifetime measured in percent.

## {% linkable_title Switch %}

The following switches are supported.

- **Boost:** Switch to manually activate boost.
