---
layout: page
title: "Amcrest IP Camera"
description: "Instructions on how to integrate Amcrest IP cameras within Home Assistant."
date: 2017-06-24 10:00
sidebar: true
comments: false
sharing: true
footer: true
logo: amcrest.png
ha_category:
  - Hub
  - Camera
  - Sensor
  - Switch
ha_iot_class: Local Polling
ha_release: 0.49
redirect_from:
  - /components/camera.amcrest/
  - /components/sensor.amcrest/
  - /components/switch.amcrest/
---

The `amcrest` camera platform allows you to integrate your [Amcrest](https://amcrest.com/) IP camera in Home Assistant.

There is currently support for the following device types within Home Assistant:

- [Camera](#camera)
- [Sensor](#sensor)
- [Switch](#switch)

## {% linkable_title Configuration %}

To enable your camera in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
amcrest:
  - host: IP_ADDRESS_CAMERA
    username: YOUR_USERNAME
    password: YOUR_PASSWORD

```

{% configuration %}
host:
  description: >
    The IP address or hostname of your camera.
    If using a hostname, make sure the DNS works as expected.
  required: true
  type: string
username:
  description: The username for accessing your camera.
  required: true
  type: string
password:
  description: The password for accessing your camera.
  required: true
  type: string
name:
  description: >
    This parameter allows you to override the name of your camera. In the case of multi-camera setups,
    this is highly recommended as camera id number will be randomly changed at each reboot if a name is not allocated.
  required: false
  type: string
  default: Amcrest Camera
port:
  description: The port that the camera is running on.
  required: false
  type: integer
  default: 80
resolution:
  description: >
    This parameter allows you to specify the camera resolution.
    For a high resolution (1080/720p), specify the option `high`.
    For VGA resolution (640x480p), specify the option `low`.
  required: false
  type: string
  default: high
stream_source:
  description: >
    The data source for the live stream. `mjpeg` will use the camera's native
    MJPEG stream, whereas `snapshot` will use the camera's snapshot API to
    create a stream from still images. You can also set the `rtsp` option to
    generate the streaming via RTSP protocol.
  required: false
  type: string
  default: snapshot
ffmpeg_arguments:
  description: >
    Extra options to pass to ffmpeg, e.g.,
    image quality or video filter options.
  required: false
  type: string
authentication:
  description: >
    Defines which authentication method to use only when **stream_source**
    is **mjpeg**. Currently, *aiohttp* only support *basic*.
  required: false
  type: string
  default: basic
scan_interval:
  description: Defines the update interval of the sensor in seconds.
  required: false
  type: integer
  default: 10
sensors:
  description: >
    Conditions to display in the frontend.
    The following conditions can be monitored:
  required: false
  type: list
  default: None
  keys:
    motion_detector:
      description: "Return `true`/`false` when a motion is detected."
    sdcard:
      description: Return the SD card usage by reporting the total and used space.
    ptz_preset:
      description: >
        Return the number of PTZ preset positions
        configured for the given camera.
switches:
  description: >
    Switches to display in the frontend.
    The following switches can be monitored:
  required: false
  type: list
  default: None
  keys:
    motion_detection:
      description: Enable/disable motion detection setting.
    motion_recording:
      description: Enable/disable recording on motion detection setting.
{% endconfiguration %}

**Note:** Amcrest cameras with newer firmware no longer have the ability to
stream `high` definition video with MJPEG encoding. You may need to use `low`
resolution stream or the `snapshot` stream source instead.  If the quality seems
too poor, lower the `Frame Rate (FPS)` and max out the `Bit Rate` settings in
your camera's configuration manager. If you defined the *stream_source* to
**mjpeg**, make sure your camera supports *Basic* HTTP authentication.
Newer Amcrest firmware may not work, then **rtsp** is recommended instead.

**Note:** If you set the `stream_source` option to `rtsp`,
make sure to follow the steps mentioned at [FFMPEG](/components/ffmpeg/)
documentation to install the `ffmpeg`.

To check if your Amcrest camera is supported/tested, visit the
[supportability matrix](https://github.com/tchellomello/python-amcrest#supportability-matrix)
link from the `python-amcrest` project.

## {% linkable_title Advanced Configuration %}

You can also use this more advanced configuration example:

```yaml
# Example configuration.yaml entry
amcrest:
  - host: IP_ADDRESS_CAMERA_1
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
    sensors:
      - motion_detector
      - sdcard
    switches:
      - motion_detection
      - motion_recording

  # Add second camera
  - host: IP_ADDRESS_CAMERA_2
    username: YOUR_USERNAME
    password: YOUR_PASSWORD
    resolution: low
    stream_source: snapshot
    sensors:
      - ptz_preset
```

## {% linkable_title Camera %}

Once you have enabled the [Amcrest component](/components/amcrest), you can add cameras to your Home Assistant configuration. add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
camera:
  - platform: amcrest
```

To check if your Amcrest camera is supported/tested, visit the [supportability matrix](https://github.com/tchellomello/python-amcrest#supportability-matrix) link from the `python-amcrest` project.

## {% linkable_title Sensor %}

Once you have enabled the [Amcrest component](/components/amcrest), you can add sensors to your Home Assistant configuration. Add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: amcrest
```

## {% linkable_title Switch %}

The `amcrest` switch platform lets you control settings of [Amcrest IP Camera](#camera) through Home Assistant.

Switches will be configured automatically. Please refer to the [component](/components/amcrest/) configuration on how to setup.

<p class='note warning'>
In previous versions, switch devices in setups with multiple cameras, would not have specific entity ID causing them to change randomly after each Home Assistant restart. The current version adds the name of the camera at the end of the switch entity ID, making it more specific and consistent and causes the name option to be required in a multi-camera system. This behavior matches the sensor behavior of the Amcrest component. Because of this, older automations may require updates to the entity ID.
</p>
