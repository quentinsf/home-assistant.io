---
layout: page
title: "Speedtest.net"
description: "How to integrate Speedtest.net within Home Assistant."
date: 2016-02-12 9:06
sidebar: true
comments: false
sharing: true
footer: true
logo: speedtest.png
ha_category:
  - System Monitor
  - Sensor
ha_release: 0.13
ha_iot_class: Cloud Polling
redirect_from:
  - /components/sensor.speedtest/
  - /components/sensor.speedtestdotnet/
---

The `speedtestdotnet` component uses the [Speedtest.net](https://speedtest.net/) web service to measure network bandwidth performance.

Enabling this component will automatically create the Speedtest.net Sensors for the monitored conditions (below).

By default, a speed test will be run every hour. The user can change the update frequency in the configuration by defining the `scan_interval` for a speed test to run.

## {% linkable_title Configuration %}

For the `server_id` check the list of [available servers](https://www.speedtest.net/speedtest-servers.php).

To add Speedtest.net sensors to your installation, add the following to your `configuration.yaml` file:

Once per hour, on the hour (default):

```yaml
# Example configuration.yaml entry
speedtestdotnet:
```

{% configuration %}
monitored_conditions:
  description: Sensors to display in the frontend.
  required: false
  default: All keys
  type: list
  keys:
    ping:
      description: "Reaction time in ms of your connection (how fast you get a response after you've sent out a request)."
    download:
      description: "The download speed (Mbit/s)."
    upload:
      description: "The upload speed (Mbit/s)."
server_id:
  description: Specify the speed test server to perform the test against.
  required: false
  type: integer
scan_interval:
  description: "Minimum time interval between updates. Supported formats: `scan_interval: 'HH:MM:SS'`, `scan_interval: 'HH:MM'` and Time period dictionary (see example below)."
  required: false
  default: 60 minutes
  type: time
manual:
  description: "`true` or `false` to turn manual mode on or off. Manual mode will disable scheduled speed tests."
  required: false
  type: boolean
  default: false
{% endconfiguration %}

### {% linkable_title Time period dictionary example %}

```yaml
scan_interval:
  # At least one of these must be specified:
  days: 0
  hours: 0
  minutes: 3
  seconds: 30
  milliseconds: 0
```

### {% linkable_title Service %}

Once loaded, the `speedtestdotnet` component will expose a service (`speedtestdotnet.speedtest`) that can be called to run a Speedtest.net speed test on demand. This service takes no parameters. This can be useful if you have enabled manual mode.

```yaml
action:
  service: speedtestdotnet.speedtest
```

This component uses [speedtest-cli](https://github.com/sivel/speedtest-cli) to gather network performance data from Speedtest.net.
Please be aware of the potential [inconsistencies](https://github.com/sivel/speedtest-cli#inconsistency) that this component may display.

When Home Assistant first starts up, the values of the speed test will show as `Unknown`. You can use the service `sensor.update_speedtest` to run a manual speed test and populate the data or just wait for the next regularly scheduled test. You can turn on manual mode to disable the scheduled speed tests.

## {% linkable_title Examples %}

In this section, you find some real-life examples of how to use this component.

### {% linkable_title Run periodically %}

Every half hour of every day:

```yaml
# Example configuration.yaml entry
speedtestdotnet:
  scan_interval:
    minutes: 30
  monitored_conditions:
    - ping
    - download
    - upload
```

### {% linkable_title Using as a trigger in an automation %}

{% raw %}
```yaml
# Example configuration.yaml entry
automation:
  - alias: "Internet Speed Glow Connect Great"
    trigger:
      - platform: template
        value_template: "{{ states('sensor.speedtest_download')|float >= 10 }}"
    action:
      - service: shell_command.green

  - alias: "Internet Speed Glow Connect Poor"
    trigger:
      - platform: template
        value_template: "{{ states('sensor.speedtest_download')|float < 10 }}"
    action:
      - service: shell_command.red
```
{% endraw %}

## {% linkable_title Notes %}

- When running on Raspberry Pi, just note that the maximum speed is limited by its 100 Mbit/s LAN adapter. The Raspberry Pi 3+ models comes with a Gigabit LAN adapter which supports a [maximum throughput](https://www.raspberrypi.org/products/raspberry-pi-3-model-b-plus/) of 300 Mbit/s.
- Running this component can have negative effects on the system's performance as it requires a fair amount of memory.
- Entries under `monitored_conditions` only control what entities are available in Home Assistant, it does not disable the condition from running.
- If ran frequently, this component has the ability to use a considerable amount of data. Frequent updates should be avoided on bandwidth-capped connections.
- While running, your network capacity is fully utilized. This may have a negative effect on other devices in use the network such as gaming consoles or streaming boxes.
