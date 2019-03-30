---
layout: page
title: "Météo-France"
description: "Instructions on how to integrate Météo-France within Home Assistant."
date: 2018-10-18 08:00
sidebar: true
comments: false
sharing: true
footer: true
logo: meteo-france.png
ha_release: 0.89
ha_iot_class: Cloud Polling
ha_category:
  - Hub
  - Sensor
  - Weather
redirect_from:
  - /components/sensor.meteo_france/
  - /components/weather.meteo_france/
---

The `meteo_france` component uses the [Météo-France](http://www.meteofrance.com/) web service as a source for meteorological data for your location. The location is based on the `city` configured in your `configuration.yaml` file.

There is currently support for the following device types within Home Assistant:

- Sensor
- Weather

It displays the current weather along with a 4 days forecast and can create sensors based on the `monitored_conditions` set in your `configuration.yaml` file.

## {% linkable_title Configuration %}

To add Météo-France to your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
meteo_france:
  - city: '76000'
```

{% configuration %}
  city:
    description: Name of the city ([see below](#about-city-configuration)).
    required: true
    type: string
  monitored_conditions:
    description: The conditions types to monitor.
    required: true
    type: list
    keys:
      temperature:
        description: The current temperature.
      weather:
        description: A human-readable text summary of the current conditions.
      wind_speed:
        description: The wind speed.
      uv:
        description: The current UV index.
      next_rain:
        description: Time to the next rain if happening for the next hour ([see note below](#about-next_rain-condition-sensor)).
      freeze_chance:
        description: Probability of temperature below 0°C for the day.
      rain_chance:
        description: Probability of rain for the day.
      snow_chance:
        description: Probability of snow for the day.
      thunder_chance:
        description: Probability of thunderstorm for the day.
{% endconfiguration %}

### {% linkable_title About `city` configuration %}

This component use the Météo-France search API to get the first city from the results returned.

It works well with french postal code, city name, etc. In case your expected result doesn't come first, you can set a more specified query like `<city name>, <postal_code>`.

It also works with international city, with mixed results. You may have to find the correct city query.
For example `Montreal, Canada` will return a city in Ardèche, France, whereas `Montreal, america` works

[http://www.meteofrance.com/mf3-rpc-portlet/rest/lieu/facet/previsions/search/montreal,amerique](http://www.meteofrance.com/mf3-rpc-portlet/rest/lieu/facet/previsions/search/montreal,amerique)

```yaml
# Example configuration.yaml entry for Montreal, Canada
meteo_france:
  - city: 'montreal,amerique'
```

### {% linkable_title About `next_rain` condition sensor %}

<p class='note warning'>
  The 1 hour rain forecast is supported for more than 75 % of metropolitan France.<br/>
  You can check if your city is covered on the [Météo-France website](http://www.meteofrance.com/previsions-meteo-france/previsions-pluie).
</p>

The `next_rain` sensor value is the time to next rain, from 0 to 55 minutes.
If no rain is forecasted for the next hour, value will be "No rain".

Attributes also give the forecast for the next hour in 5 minutes intervals.
Possible value for each intervals attributes are:

- 1 No rain
- 2 Light rain
- 3 Moderate rain
- 4 Heavy rain

### {% linkable_title Complete example %}

This is an example for 3 cities forecast with different monitored conditions:

```yaml
# Complete example configuration.yaml entry
meteo_france:
  - city: 'Rouen'
    monitored_conditions:
        - rain_chance
        - freeze_chance
        - thunder_chance
        - snow_chance
        - weather
        - next_rain
        - wind_speed
        - temperature
        - uv
  - city: 'Oslo, norvege'
    monitored_conditions:
      - temperature
  - city: '51100'
```
