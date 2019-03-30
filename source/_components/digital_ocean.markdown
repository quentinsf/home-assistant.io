---
layout: page
title: "Digital Ocean"
description: "Instructions on how to integrate the Digital Ocean within Home Assistant."
date: 2016-09-24 20:00
sidebar: true
comments: false
sharing: true
footer: true
ha_category:
  - System Monitor
  - Binary Sensor
  - Switch
ha_release: "0.30"
logo: digital_ocean.png
ha_iot_class: Local Polling
redirect_from:
  - /components/binary_sensor.digital_ocean/
  - /components/switch.digital_ocean/
---

The `digital_ocean` component allows you to access the information about your [Digital Ocean](https://www.digitalocean.com/) droplets from Home Assistant.

There is currently support for the following device types within Home Assistant:

- [Binary Sensor](/components/digital_ocean/#binary-sensor)
- [Switch](/components/digital_ocean/#switch)

## {% linkable_title Setup %}

Obtain your API key from your [Digital Ocean dashboard](https://cloud.digitalocean.com/settings/api/tokens).

## {% linkable_title Configuration %}

To integrate your Digital Ocean droplets with Home Assistant, add the following section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
digital_ocean:
  access_token: YOUR_API_KEY
```

{% configuration %}
access_token:
  description: Your Digital Ocean API access token.
  required: true
  type: string
{% endconfiguration %}

## {% linkable_title Binary Sensor %}

The `digital_ocean` binary sensor platform allows you to monitor your Digital Ocean droplets.

### {% linkable_title Configuration %}

To use your Digital Ocean droplets, you first have to set up your [Digital Ocean hub](/components/digital_ocean/) and then add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
  - platform: digital_ocean
    droplets:
      - 'fedora-512mb-nyc3-01'
      - 'coreos-512mb-nyc3-01'
```

{% configuration %}
droplets:
  description: List of droplets you want to monitor.
  required: true
  type: list
{% endconfiguration %}

## {% linkable_title Switch %}

The `digital_ocean` switch platform allows you to control (start/stop) your Digital Ocean droplets.

### {% linkable_title Configuration %}

To use your Digital Ocean droplets, you first have to set up your [Digital Ocean hub](/components/digital_ocean/) and then add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
switch:
  - platform: digital_ocean
    droplets:
      - 'fedora-512mb-nyc3-01'
      - 'coreos-512mb-nyc3-01'
```

{% configuration %}
droplets:
  description: List of droplets you want to control.
  required: true
  type: list
{% endconfiguration %}
