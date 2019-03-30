---
layout: page
title: "Ring"
description: "Instructions on how to integrate your Ring.com devices within Home Assistant."
date: 2017-04-01 10:00
sidebar: true
comments: false
sharing: true
footer: true
logo: ring.png
ha_category:
  - Doorbell
  - Binary Sensor
  - Camera
  - Sensor
ha_release: 0.42
ha_iot_class: Cloud Polling
redirect_from:
  - /components/binary_sensor.ring/
  - /components/camera.ring/
  - /components/sensor.ring/
---

The `ring` implementation allows you to integrate your [Ring.com](https://ring.com/) devices in Home Assistant.

There is currently support for the following device types within Home Assistant:

- [Binary Sensor](#binary-sensor)
- [Camera](#camera) - downloading and playing Ring video will require a Ring Protect plan.
- [Sensor](#sensor)

Currently only doorbells are supported by this sensor.

## {% linkable_title Configuration %}

To enable device linked in your [Ring.com](https://ring.com/) account, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
ring:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
```

{% configuration %}
username:
  description: The username for accessing your Ring account.
  required: true
  type: string
password:
  description: The password for accessing your Ring account.
  required: true
  type: string
{% endconfiguration %}

## {% linkable_title Binary Sensor %}

Once you have enabled the [Ring component](/components/ring), you can start using a binary sensor. Add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
  - platform: ring
```

{% configuration %}
monitored_conditions:
  description: Conditions to display in the frontend. The following conditions can be monitored. If not specified, all conditions below will be enabled.
  required: false
  type: list
  keys:
    ding:
      description: Return a boolean value when the doorbell button was pressed.
    motion:
      description: Return a boolean value when a movement was detected by the Ring doorbell.
{% endconfiguration %}

Currently it supports doorbell, external chimes and stickup cameras.

## {% linkable_title Camera %}

<p class='note'>
Please note that downloading and playing Ring video will require a Ring Protect plan.
</p>

Once you have enabled the [Ring component](/components/ring), you can start using the camera platform. Add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
camera:
  - platform: ring
```

{% configuration %}
ffmpeg_arguments:
  description: Extra options to pass to ffmpeg, e.g., image quality or video filter options.
  required: false
  type: string
scan_interval:
  description: How frequently to query for new video in seconds.
  required: false
  type: integer
  default: 90
{% endconfiguration %}

**Note:** To be able to playback the last capture, it is required to install the `ffmpeg` component. Make sure to follow the steps mentioned at [FFMPEG](/components/ffmpeg/) documentation.

Currently it supports doorbell and stickup cameras.

## {% linkable_title Saving the videos captured by your Ring Door Bell %}

You can save locally the latest video captured by your Ring Door Bell using the [downloader](/components/downloader) along with either an [automation](/components/automation) or [python_script](/components/python_script). First, enable the [downloader](/components/downloader) component in your configuration by adding the following to your `configuration.yaml`.

```yaml
downloader:
  download_dir: downloads
```

Then you can use the following `action` in your automation (this will save the video file under `<config>/downloads/ring_<camera_name>/`):

```yaml
action:
  - service: downloader.download_file
    data_template:
      url: "{{ states.camera.front_door.attributes.video_url }}"
      subdir: "{{states.camera.front_door.attributes.friendly_name}}"
      filename: "{{states.camera.front_door.attributes.friendly_name}}"
```

If you want to use `python_script`, enable it your `configuration.yaml` file first:

```yaml
python_script:
```

You can then use the following `python_script` to save the video file:

```python
# obtain ring doorbell camera object
# replace the camera.front_door by your camera entity
ring_cam = hass.states.get('camera.front_door')

subdir_name = 'ring_{}'.format(ring_cam.attributes.get('friendly_name'))

# get video URL
data = {
    'url': ring_cam.attributes.get('video_url'),
    'subdir': subdir_name,
    'filename': ring_cam.attributes.get('friendly_name')
}

# call downloader component to save the video
hass.services.call('downloader', 'download_file', data)
```

## {% linkable_title Sensor %}

Once you have enabled the [Ring component](/components/ring), you can start using the sensor platform. Add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: ring
```

{% configuration %}
monitored_conditions:
  type: list
  required: false
  description: Conditions to display in the frontend. The following conditions can be monitored. If not specified, all conditions below will be enabled.
  keys:
    battery:
       description: Return the battery level from device.
    last_activity:
       description: Return the timestamp from the last event captured (ding/motion/on demand) by the Ring doorbell camera.
    last_ding:
       description: Return the timestamp from the last time the Ring doorbell button was pressed.
    last_motion:
       description: Return the timestamp from the last motion event captured by the Ring doorbell camera.
    volume:
       description: Return the volume level from the device.
    wifi_signal_category:
       description: Return the WiFi signal level from the device.
    wifi_signal_strength:
       description: Return the WiFi signal strength (dBm) from the device.
{% endconfiguration %}

Currently it supports doorbell, external chimes and stickup cameras.
