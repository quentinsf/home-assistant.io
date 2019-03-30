---
layout: page
title: "Dweet.io"
description: "Transfer events to Dweet.io."
date: 2016-05-07 07:08
sidebar: true
comments: false
sharing: true
footer: true
logo: dweet.png
ha_category:
  - History
  - Sensor
ha_release: 0.19
ha_iot_class: Cloud Polling
redirect_from:
  - /components/sensor.dweet/
---

The `dweet` component makes it possible to transfer details collected with Home Assistant to [Dweet.io](http://dweet.io/) and visualize them with [freeboard.io](https://freeboard.io). Keep in mind that your information will be public!

<p class='img'>
  <img src='{{site_root}}/images/screenshots/dweet-freeboard.png' />
</p>

<p class='note warning'>
  The publishing interval is limited to 1 second. This means that it's possible to miss fast changes.
</p>

There is currently support for the following device types within Home Assistant:

- [Sensor](#sensor)

## {% linkable_title Configuration %}

To use the `dweet` component in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
dweet:
  name: YOUR_UNIQUE_IDENTIFIER
  whitelist:
    - input_number.brightness
    - input_boolean.notify_home
    - sensor.weather_temperature
    - sensor.cpu
```

{% configuration %}
name:
  description: A unique identifier for your Home Assistant instance.
  required: true
  type: string
whitelist:
  description: List of entity IDs you want to publish
  required: true
  type: list
{% endconfiguration %}

## {% linkable_title Sensor %}

The `dweet` sensor platform allows you to get details from your devices which are publishing their values to [Dweet.io](https://dweet.io/).

### {% linkable_title Configuration %}

To use Dweet.io sensors in your installation, add the following to your `configuration.yaml` file:

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: dweet
    device: THING_NAME
    value_template: '{{ value_json.VARIABLE }}'
```
{% endraw %}

{% configuration %}
device:
  description: Identification of the device (also known as `thing`).
  required: true
  type: string
value_template:
  description: The variable to extract a value from the content.
  required: true
  type: template
name:
  description: Let you overwrite the name of the device in the frontend.
  required: false
  default: Dweet.io Sensor
  type: string
unit_of_measurement:
  description: Defines the unit of measurement of the sensor, if any.
  required: false
  type: string
{% endconfiguration %}

### {% linkable_title Full configuration sample %}

A full configuration entry could look like the sample below.

{% raw %}
```yaml
# Example configuration.yaml entry
sensor:
  - platform: dweet
    name: Temperature
    device: THING_NAME
    value_template: '{{ value_json.VARIABLE }}'
    unit_of_measurement: "°C"
```
{% endraw %}

### {% linkable_title Interacting with Dweet.io %}

You can easily send dweets from the command-line to test your sensor with `curl`.

```bash
$ curl -H 'Content-Type: application/json' -d '{"temperature": 40, "humidity": 65}' https://dweet.io/dweet/for/ha-sensor
```

will give you a response like the one below:

```json
{"this":"succeeded","by":"dweeting","the":"dweet","with":{"thing":"ha-sensor","created":"2015-12-10T09:43:31.133Z","content":{"temperature":40,"humidity":65}}}
```

The [dweepy](https://github.com/paddycarey/dweepy) module gives you another option to work with [Dweet.io](https://dweet.io/).

Send a dweet.

```bash
$ python3
>>> import dweepy
>>> dweepy.dweet_for('ha-sensor', {'temperature': '23', 'humiditiy':'81'})
{'thing': 'ha-sensor', 'created': '2015-12-10T09:46:08.559Z', 'content': {'humiditiy': 81, 'temperature': 23}}
```

Receive the latest dweet.

```bash
>>> dweepy.get_latest_dweet_for('ha-sensor')
[{'thing': 'ha-sensor'', 'created': '2015-12-10T09:43:31.133Z', 'content': {'humidity': 65, 'temperature': 40}}]
```