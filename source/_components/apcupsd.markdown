---
layout: page
title: "APCUPSd"
description: "Instructions on how to integrate APCUPSd status with Home Assistant."
date: 2016-02-10 17:11
sidebar: true
comments: false
sharing: true
footer: true
logo: apcupsd.png
ha_category:
  - System Monitor
  - Binary Sensor
  - Sensor
ha_release: 0.13
ha_iot_class: Local Polling
redirect_from:
  - /components/binary_sensor.apcupsd/
  - /components/sensor.apcupsd/
---

[APCUPSd](http://www.apcupsd.org/) status information can be integrated into Home Assistant when the Network Information Server (NIS) [is configured](http://www.apcupsd.org/manual/manual.html#nis-server-client-configuration-using-the-net-driver) is enabled on the APC device.

There is currently support for the following device types within Home Assistant:

- [Binary Sensor](#binary-sensor)
- [Sensor](#sensor)

## {% linkable_title Configuration %}

To enable this sensor, add the following lines to your `configuration.yaml`:

```yaml
# Example configuration.yaml entry
apcupsd:
```

{% configuration %}
host:
  description: The hostname/IP address on which the APCUPSd NIS is being served.
  required: false
  type: string
  default: localhost
port:
  description: The port on which the APCUPSd NIS is listening.
  required: false
  type: integer
  default: 3551
{% endconfiguration %}

<p class='note'>
If you get `ConnectionRefusedError: Connection refused` errors in the Home assistant logs, ensure the [APCUPSd](http://www.apcupsd.org/) configuration directives used by its Network Information Server is set to permit connections from all addresses [NISIP 0.0.0.0](http://www.apcupsd.org/manual/manual.html#configuration-directives-used-by-the-network-information-server), else non-local addesses will not connect. This includes Hass.io running in Docker, even when hosted on the same machine or a virtual machine.
 </p>

## {% linkable_title Binary sensor %}

In addition to the [APCUPSd Sensor](#sensor) devices, you may also create a device which is simply "on" when the UPS status is online and "off" at all other times.

### {% linkable_title Configuration %}

To enable this sensor, you first have to set up apcupsd component (above), and add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
binary_sensor:
  - platform: apcupsd
```

{% configuration %}
name:
  description: Name to use in the frontend.
  required: false
  type: string
  default: UPS Online Status
{% endconfiguration %}

## {% linkable_title Sensor %}

 The `apcupsd` sensor platform allows you to monitor a UPS (battery backup) by using data from the [apcaccess](http://linux.die.net/man/8/apcaccess) command.

### {% linkable_title Configuration %}

To use this sensor platform, you first have to set up apcupsd component (above), and add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
sensor:
  - platform: apcupsd
    resources:
      - bcharge
      - linev
```

{% configuration %}
resources:
  description: Contains all entries to display.
  required: true
  type: list
{% endconfiguration %}

### {% linkable_title Example  %}

Given the following output from `apcaccess`:

```yaml
APC      : 001,051,1149
DATE     : 2016-02-09 17:13:31 +0000
HOSTNAME : localhost
VERSION  : 3.14.12 (29 March 2014) redhat
UPSNAME  : netrack
CABLE    : Custom Cable Smart
DRIVER   : APC Smart UPS (any)
UPSMODE  : Stand Alone
STARTTIME: 2016-02-09 16:06:47 +0000
MODEL    : SMART-UPS 1400
STATUS   : TRIM ONLINE
LINEV    : 247.0 Volts
LOADPCT  : 13.0 Percent
BCHARGE  : 100.0 Percent
TIMELEFT : 104.0 Minutes
MBATTCHG : 5 Percent
MINTIMEL : 3 Minutes
MAXTIME  : 0 Seconds
MAXLINEV : 249.6 Volts
MINLINEV : 244.4 Volts
OUTPUTV  : 218.4 Volts
[...]
```

Use the (case insensitive) values from the left hand column:

```yaml
sensor:
  - platform: apcupsd
    resources:
      - linev
      - loadpct
      - timeleft
```
