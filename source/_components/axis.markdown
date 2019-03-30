---
layout: page
title: "Axis"
description: "Integration between network devices from Axis Communications with Home Assistant."
date: 2017-04-30 23:04
sidebar: true
comments: false
sharing: true
footer: true
logo: axis.png
ha_category:
  - Camera
  - Binary Sensor
ha_config_flow: true
ha_release: 0.45
ha_iot_class: Local Push
redirect_from:
  - /components/binary_sensor.axis/
  - /components/camera.axis/
---

[Axis Communications](https://www.axis.com/) devices are surveillance cameras, speakers, access control and other security-related network connected hardware. Event API works with firmware 5.50 and newer.

Home Assistant will automatically discover their presence on your network.

## {% linkable_title Configuration %}

For configuration go to the `Integrations pane` on your Home Assistant instance.

<p class='note'>
  It is recommended that you create a user on your Axis device specifically for Home Assistant. For all current functionality, it is enough to create a user belonging to user group viewer.
</p>

## {% linkable_title Troubleshooting discovery %}

If your device is not discovered. On your camera, go to **System Options** -> **Advanced** -> **Plain Config**. Change the drop-down box to `network` and click `Select Group`. If `Network Interface I0 ZeroConf` contains the `169.x.x.x` IP address, unchecked the box next to `Enabled` for this section and click `Save`.

## {% linkable_title Binary Sensor %}

The following sensor types are supported:

- Motion detection (VMD3/VMD4)
- Passive IR motion detection
- Sound detection
- Day/night mode
