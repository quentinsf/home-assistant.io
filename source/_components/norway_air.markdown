---
layout: page
title: "Norway Air Quality"
description: "Display the current status of Norway air quality."
date: 2019-02-02 18:00
sidebar: true
comments: false
sharing: true
footer: true
logo: metno.png
ha_category: Health
ha_iot_class: Cloud Polling
ha_release: 0.88
---

The `norway_air` component [queries](https://luftkvalitet.miljostatus.no/) the Norway air quality [data feed](https://api.met.no/weatherapi/airqualityforecast/0.1/documentation) provided by the Norwegian Meteorological Institute.

To add air quality sensor to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
air_quality:
  - platform: norway_air
```

{% configuration %}
name:
  description: Additional name for the sensor.
  required: false
  type: string
  default: Air quality
forecast:
  description: If you want to get forecast data instead of the current data, set this to the number of hours that you want to look into the future.
  required: false
  type: integer
latitude:
  description: Manually specify latitude.
  required: false
  type: number
  default: Provided by Home Assistant configuration
longitude:
  description: Manually specify longitude.
  required: false
  type: number
  default: Provided by Home Assistant configuration
{% endconfiguration %}
