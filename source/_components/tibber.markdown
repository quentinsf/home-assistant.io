---
layout: page
title: "Tibber"
description: "Instructions on how to integrate Tibber within Home Assistant."
date: 2015-10-04 16:34
sidebar: true
comments: false
sharing: true
footer: true
logo: tibber.png
ha_category:
  - Energy
  - Sensor
  - Notifications
ha_release: 0.80
ha_qa_scale: silver
ha_iot_class: Cloud Polling
redirect_from:
  - /components/notify.tibber/
  - /components/sensor.tibber/
---

The `tibber` component provides a sensor with the current electricity price if you are a [Tibber](https://tibber.com/) customer.
If you have a [Tibber Pulse](https://norge.tibber.com/products/pulse/) or [Watty](https://watty.io/) it will also show the electricity consumption in real time.

There is currently support for the following device types within Home Assistant:

- [Notifications](#notifications)
- [Sensor](#sensor)

## {% linkable_title Setup %}

Go to [developer.tibber.com/](https://developer.tibber.com/) to get your API token.

## {% linkable_title Configuration %}

To add Tibber to your installation, add the following to your `configuration.yaml` file:

```yaml
tibber:
  access_token: YOUR_ACCESS_TOKEN
```

{% configuration %}
access_token:
  description: Your Tibber API token.
  required: true
  type: string
{% endconfiguration %}

## {% linkable_title Notifications %}

Tibber can send a notification by calling the [`notify` service](/components/notify/). It will send a notification to all devices registered in the Tibber account.

The requirement is that you have setup the [`tibber` component](#setup).
To use notifications, please see the [getting started with automation page](/getting-started/automation/).

### {% linkable_title Send message %}

```yaml
action:
  service: notify.tibber
  data:
    title: Your title
    message: This is a message for you!
```

## {% linkable_title Sensor %}

The `tibber` sensor provides the current electricity price if you are a [Tibber](https://tibber.com/) customer.
If you have a Tibber Pulse it will also show the electricity consumption in real time.

The requirement is that you have setup the [`tibber` component](#setup). The sensor will show once the transfer date to tibber has been confirmed.

## {% linkable_title Examples %}

In this section, you will find some real-life examples of how to use this sensor.

### {% linkable_title Electricity price %}

The electricity price can be used to make automations. The sensor has a `max_price` and `min_price` attribute, with max and min price for the current day. Here is an example to get a notification when the price is above 90% of the maximum price for the day:

{% raw %}

```yaml
- alias: "Electricity price"
  trigger:
    platform: time_pattern
  # Matches every hour at 1 minutes past whole
    minutes: 1
  condition:
    condition: template
    value_template: '{{ float(states.sensor.electricity_price_hamretunet_10.state) > 0.9 * float(states.sensor.electricity_price_hamretunet_10.attributes.max_price) }}'
  action:
   - service: notify.pushbullet
     data:
       title: "Electricity price"
       target: "device/daniel_telefon_cat"
       message: "The electricity price is now {{ states.sensor.electricity_price_hamretunet_10.state }}"
```

{% endraw %}
