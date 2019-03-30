---
layout: page
title: "Rachio"
description: "Instructions on how to use Rachio with Home Assistant."
date: 2018-06-23 16:04
sidebar: true
comments: false
sharing: true
footer: true
logo: rachio.png
ha_category:
  - Irrigation
  - Binary Sensor
  - Switch
ha_iot_class: Cloud Push
ha_release: 0.73
redirect_from:
  - /components/binary_sensor.rachio/
  - /components/switch.rachio/
---

The `rachio` platform allows you to control your [Rachio irrigation system](http://rachio.com/).

There is currently support for the following device types within Home Assistant:

- **Binary Sensor** - Allows you to view the status of your [Rachio irrigation system](http://rachio.com/).
- [**Switch**](#switch)

They will be automatically added if the Rachio component component is loaded.

## {% linkable_title Getting your Rachio API Key %}

1. Log in at [https://app.rach.io/](https://app.rach.io/).
2. Click the "Account Settings" menu item at the bottom of the left sidebar
3. Click "Get API Key"
4. Copy the API key from the dialog that opens.

## {% linkable_title Configuration %}

To add this platform to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
rachio:
  api_key: YOUR_API_KEY
```

{% configuration %}
api_key:
  description: The API key for the Rachio account.
  required: true
  type: string
hass_url_override:
  description: If your instance is unaware of its actual web location (`base_url`).
  required: false
  type: string
manual_run_mins:
  description: For how long, in minutes, to turn on a station when the switch is enabled.
  required: false
  default: 10
  type: integer
{% endconfiguration %}

<p class='note'>
**Water-saving suggestion:**<br>
Set `manual_run_mins` to a high maximum failsafe value when using scripts to control zones. If something goes wrong with your script, Home Assistant, or you hit the Rachio API rate limit of 1700 calls per day, the controller will still turn off the zone after this amount of time.
</p>

### {% linkable_title iFrame %}

If you would like to see and control more detailed zone information, create an [iFrame](/components/panel_iframe/) that renders the Rachio web app.

```yaml
panel_iframe:
  rachio:
    title: Rachio
    url: "https://app.rach.io"
    icon: mdi:water-pump
```

## {% linkable_title Switch %}

The `rachio` switch platform allows you to toggle zones connected to your [Rachio irrigation system](http://rachio.com/) on and off.

Once configured, a switch will be added for every zone that is enabled on every controller in the account provided, as well as a switch to toggle each controller's standby mode.

## {% linkable_title Examples %}

In this section, you find some real-life examples of how to use this switch.

### {% linkable_title `groups.yaml` example %}

```yaml
irrigation:
  name: Irrigation
  icon: mdi:water-pump
  view: true
  entities:
  - group.zones_front
  - group.zones_back
  - switch.side_yard

zones_front:
  name: Front Yard
  view: false
  entities:
  - switch.front_bushes
  - switch.front_yard

zones_back:
  name: Back Yard
  view: false
  entities:
  - switch.back_garden
  - switch.back_porch
```
