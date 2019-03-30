---
layout: page
title: "Hive"
description: "Instructions on how to integrate Hive devices with Home Assistant."
date: 2017-09-24 21:00
sidebar: true
comments: false
sharing: true
footer: true
logo: hive.png
ha_category:
  - Hub
  - Binary Sensor
  - Climate
  - Light
  - Sensor
  - Switch
ha_release: 0.59
ha_iot_class: Cloud Polling
redirect_from:
  - /components/binary_sensor.hive/
  - /components/climate.hive/
  - /components/light.hive/
  - /components/sensor.hive/
  - /components/switch.hive/
---

The `hive` component is the main component to set up and integrate all supported Hive devices. Once configured with the minimum required details it will detect and add all your Hive devices into Home Assistant, including support for multizone heating.

This component uses the unofficial API used in the official Hive website [https://my.hivehome.com](https://my.hivehome.com), and you will need to use the same Username and Password you use on the Hive website to configure this Hive component in Home Assistant.

There is currently support for the following device types within Home Assistant:

- [Binary Sensor](#binary-sensor)
- [Climate](#climate)
- [Light](#light)
- [Sensor](#sensor)
- [Switch](#switch)

To add your Hive devices into your Home Assistant installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
hive:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
```

{% configuration %}
username:
  description: Your username from [https://my.hivehome.com](https://my.hivehome.com).
  required: true
  type: string
password:
  description: Your password from [https://my.hivehome.com](https://my.hivehome.com).
  required: true
  type: string
scan_interval:
  description: The time in minutes between Hive API calls
  required: false
  type: integer
  default: 2
{% endconfiguration %}

## {% linkable_title Binary Sensor %}

The `hive` binary sensor component integrates your Hive sensors into Home Assistant.

The platform supports the following Hive products:

- Hive Window or Door Sensor
- Hive Motion Sensor

## {% linkable_title Climate %}

The `hive` climate platform integrates your Hive thermostat and hot water into Home Assistant, enabling control of setting the **mode** and setting the **target temperature**.

A short boost for Hive Heating or Hive Hot water can be set by using the **Aux Heat** function, this will turn on the boost feature for Hive Heating or Hive Hot water for 30 minutes at 0.5 degrees higher than the current temperature.

The platform supports the following Hive products:

- Hive Active Heating
- Hive Multizone
- Hot water control

## {% linkable_title Light %}

The `hive` light platform integrates your Hive lights into Home Assistant, enabling control of various settings, depending on the model light.

The platform supports the following Hive products:

- Hive Active Light Dimmable
- Hive Active Light Cool to Warm White
- Hive Active Light Color Changing

## {% linkable_title Sensor %}

The `hive` sensor component exposes Hive data as a sensor.

The platform exposes the following sensors:

- Hive Hub Online Status
- Hive Outside Temperature

## {% linkable_title Switch %}

The `hive` switch platform integrates your Hive plugs into Home Assistant, enabling control of your devices.

The platform supports the following Hive products:

- Hive Active Plug
