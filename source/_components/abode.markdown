---
layout: page
title: "Abode Home Security"
description: "Instructions on integrating Abode home security with Home Assistant."
date: 2017-08-26 0:28
sidebar: true
comments: false
sharing: true
footer: true
logo: abode.jpg
ha_category:
  - Hub
  - Alarm
  - Binary Sensor
  - Camera
  - Cover
  - Light
  - Lock
  - Sensor
  - Switch
ha_release: 0.52
ha_iot_class: Cloud Push
redirect_from:
  - /components/alarm_control_panel.abode/
  - /components/binary_sensor.abode/
  - /components/camera.abode/
  - /components/cover.abode/
  - /components/lock.abode/
  - /components/light.abode/
  - /components/switch.abode/
  - /components/sensor.abode/
---

The `abode` component will allow users to integrate their Abode Home Security systems into Home Assistant and use its alarm system and sensors to automate their homes.

Please visit the [Abode website](https://goabode.com/) for further information about Abode Security.

There is currently support for the following device types within Home Assistant:

- **Alarm Control Panel**: Reports on the current alarm status and can be used to arm and disarm the system.
- [**Binary Sensor**](/components/abode/#binary-sensor): Reports on `Quick Actions`, `Door Contacts`, `Connectivity` sensors (remotes, keypads, and status indicators), `Moisture` sensors, and `Motion` or `Occupancy` sensors.
- **Camera**: Reports on `Camera` devices and will download and show the latest captured still image.
- **Cover**: Reports on `Secure Barriers` and can be used to open and close the cover.
- **Lock**: Reports on `Door Locks` and can be used to lock and unlock the door.
- [**Light**](/components/abode/#light): Reports on `Dimmer` lights and can be used to dim or turn the light on and off.
- [**Switch**](/components/abode/#switch): Reports on `Power Switch` devices and can be used to turn the power switch on and off. Also reports on `Automations` set up in the Abode system and allows you to activate or deactivate them.
- **Sensor**: Reports on `Temperature`, `Humidity`, and `Light` sensors.

## {% linkable_title Configuration %}

To use Abode devices in your installation,
add the following `abode` section to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
abode:
  username: abode_username
  password: abode_password
  name: Abode Alarm System
  polling: false
  exclude:
    - 'ZW:0000000034'
    - 'RF:00000011'
  lights:
    - 'ZW:0000000022'
```

{% configuration %}
username:
  description: Username for your Abode account.
  required: true
  type: string
password:
  description: Password for your Abode account.
  required: true
  type: string
name:
  description: The name for your alarm controller.
  required: false
  type: string
polling:
  description: >
    Enable polling if cloud push updating is less reliable.
    Will update the devices once every 30 seconds.
  required: false
  type: boolean
  default: false
exclude:
  description: >
    A list of devices to exclude from Home Assistant by their Abode `device_id`
    or `automation_id`, found within the component attributes.
  required: false
  type: list
lights:
  description: >
    A list of switch devices that Home Assistant should treat as lights by the
    switches Abode `device_id`, found within the component attributes.
  required: false
  type: list
{% endconfiguration %}

## {% linkable_title Events %}

There are a number of events that can be triggered from Abode.
They are grouped into the below events:

- **abode_alarm**: Fired when an alarm event is triggered from Abode. This includes Smoke, CO, Panic, and Burglar alarms.
- **abode_alarm_end**: Fired when an alarm end event is triggered from Abode.
- **abode_automation**: Fired when an Automation is triggered from Abode.
- **abode_panel_fault**: Fired when there is a fault with the Abode hub. This includes events like loss of power, low battery, tamper switches, polling failures, and signal interference.
- **abode_panel_restore**: Fired when the panel fault is restored.

All events have the fields:

Field | Description
----- | -----------
`device_id` | The Abode device ID of the event.
`device_name` | The Abode device name of the event.
`device_type` | The Abode device type of the event.
`event_code` | The event code of the event.
`event_name` | The name of the event.
`event_type` | The type of the event.
`event_utc` | The UTC timestamp of the event.
`user_name` | The Abode user that triggered the event, if applicable.
`date` | The date of the event in the format `MM/DD/YYYY`.
`time` | The time of the event in the format `HH:MM AM`.

There is a unique list of known event_codes that can be found
[here](https://github.com/MisterWil/abodepy/files/1262019/timeline_events.txt).

## {% linkable_title Services %}

### {% linkable_title Service `change_setting` %}

Change settings on your Abode system.
For a full list of settings and valid values, consult the
[AbodePy settings section](https://github.com/MisterWil/abodepy/blob/master/README.rst#settings).

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `setting` | No | The setting you wish to change.
| `value` | No | The value you wish to change the setting to.

### {% linkable_title Service `capture_image` %}

Request a new still image from your Abode IR camera.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | No | String or list of strings that point at `entity_id`s of Abode cameras.

### {% linkable_title Service `trigger_quick_action` %}

Trigger a quick action automation on your Abode system.

| Service data attribute | Optional | Description |
| ---------------------- | -------- | ----------- |
| `entity_id` | No | String or list of strings that point at `entity_id`s of binary_sensors that represent your Abode quick actions.

### {% linkable_title Binary Sensor %}

This component will add `Door Contacts`, `Connectivity` sensors (remotes, keypads, and status indicators), `Moisture` sensors, and `Motion` or `Occupancy` sensors.

This component will also list all Abode `Quick Actions` that are set up. You can trigger these quick actions by passing the `entity_id` of your quick action binary sensor to the [trigger_quick_action service](/components/abode/#trigger_quick_action).

### {% linkable_title Light %}

This component will automatically add `Lights` configured in your Abode account. You can reclassify `Switches` to show up within Home Assistant as lights by listing the Abode device ID in your [configuration](/components/abode/#configuration).

### {% linkable_title Switch %}

This component will automatically add `Power Switches` configured in your Abode account. You can reclassify switches to show up within Home Assistant as `Lights` by listing the Abode device ID in your [configuration](/components/abode/#configuration).

This component will also list all Abode `Automations` that are set up within the Abode system, allowing you to activate and deactivate the automations.
