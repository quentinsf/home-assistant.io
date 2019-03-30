---
layout: page
title: "Nissan Leaf"
description: "Instructions for how to integrate Nissan Leaf(s) into Home Assistant."
date: 2019-01-05 07:00
sidebar: true
comments: false
sharing: true
footer: true
logo: nissan.png
ha_category: Car
ha_release: 0.89
ha_iot_class: Cloud Polling
redirect_from:
 - /components/binary_sensor.nissan_leaf/
 - /components/sensor.nissan_leaf/
 - /components/switch.nissan_leaf/
---

The `nissan_leaf` component offers integration with the [NissanConnect EV](http://youplus.nissan.co.uk/GB/en/YouPlus/ConnectedServices.html) cloud service. NissanConnect EV was previously known as Nissan Carwings. It offers:

* sensors for the battery status, range and charging status
* a switch to start and stop the climate control
* a device tracker to locate the car (only on later Leaf models)
* services to request updates from the car and to request the car starts charging.

## {% linkable_title Configuration %}

To use Nissan Leaf in your installation, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
nissan_leaf:
  username: "YOUR_USERNAME"
  password: "YOUR_PASSWORD"
  region: "YOUR_REGION"
```
{% configuration %}
username:
  description: The username associated with your NissanConnect EV account. Enclose in quotes.
  required: true
  type: string
password:
  description: The password for your given NissanConnect EV account. Enclose in quotes.
  required: true
  type: string
region:
  description: The region where the NissanConnect EV account is registered. Should be one of the following, NE (for Europe), NNA (USA), NCI (Canada), NMA (Australia), NML (Japan).
  required: true
  type: string
nissan_connect:
  description: If your car has the updated head unit (NissanConnect rather than Carwings) then the location can be aquired and exposed via a device tracker. If you have Carwings then this should be set to false. The easiest way to identify NissanConnect is if the T&C buttons are orange and blue, for CarWings they're both blue, or just look for anything saying "CarWings" in Settings area of the infotainment system.
  required: false
  type: boolean
update_interval:
  description: The interval between updates if the climate control is off and the car is not charging. Set in any time unit (e.g. minutes, hours, days!).
  required: false
  default: 1 hour
  type: time
update_interval_charging:
  description: The interval in minutes between updates if charging.
  required: false
  default: 15
  type: time
update_interval_climate:
  description: The interval in minutes between updates if climate control on.
  required: false
  default: 5
  type: time
{% endconfiguration %}

## {% linkable_title Full configuration sample %}

A more advanced example for setting the update interval:

```yaml
# Example configuration.yaml entry
nissan_leaf:
  username: "YOUR_USERNAME"
  password: "YOUR_PASSWORD"
  region: "YOUR_REGION"
  nissan_connect: true
  update_interval:
    hours: 1
  update_interval_charging:
    minutes: 15
  update_interval_climate:
    minutes: 5
  force_miles: true
```

## {% linkable_title Starting a Charge %}

You can use the `nissan_leaf.start_charge` service to send a request to the Nissan servers to start a charge. The car must be plugged in!  The service requires you to provide the vehicle identification number (VIN) as a parameter. You can see the VIN on the attributes of all the entities created by this component, except the device_tracker.

```yaml
- service: nissan_leaf.start_charge
  data:
    vin: '1HGBH41JXMN109186'             # replace
```

## {% linkable_title Updating on-demand using Automation %}

You can also use the `nissan_leaf.update` service to request an on-demand update. To update almost exclusively via the service set the `update_interval` to a high value in the component configuration.  The service requests the VIN number as described above.

```yaml
- id: update_when_driver_not_home
  alias: 'Update when driver not home'
  initial_state: on
  trigger:
    - platform: time_pattern
      minutes: '/30'
  condition:
    - condition: state
      entity_id: device_tracker.drivername   # replace
      state: 'not_home'
  action:
    - service: nissan_leaf.update
      data:
        vin: '1HGBH41JXMN109186'             # replace
```

## {% linkable_title Hints %}

* The update interval has a minimum of two minutes.
* Requesting updates uses a small amount of power from the 12 V battery. The 12 V battery charges from the main traction battery when the car is not plugged in. If the car is left plugged in for a long time, or if the main traction battery is very low then the 12 V battery may gradually discharge. A low update interval may cause the 12 V battery to become flat.  When the 12 V battery is flat the car will not start. _Do not set the update interval too low.  Use at your own risk._
* This component communicates with the Nissan Servers which then communicate with the car. The communication between the car and the Nissan Servers is very slow, and takes up to five minutes to get information from the car, therefore the default polling interval is set to one hour to not overwhelm the connection.
* Responses from the Nissan servers are received separately for the battery/range, climate control and location. The `updated_on` attribute will show the last time the data was retrieved from the server. There are separate attributes for when the `next_update` is scheduled, and to indicate if `update_in_progress`. The `nissan_leaf.update` service will reset the `next_update` attribute.
* The Nissan APIs do not allow charging to be stopped remotely.
* The Nissan servers have a history of being unstable, therefore please confirm that the official Nissan Leaf app/website is working correctly before reporting bugs.
* In the UK the cut-off for Carwings was the 16 plate 24 kWh and the 65 plate 30 kWh. Cars after this have NissanConnect.

Please report bugs using the following logger configuration.

```yaml
logger:
  default: critical
  logs:
    homeassistant.components.nissan_leaf: debug
    homeassistant.components.device_tracker.nissan_leaf: debug
    homeassistant.components.sensor.nissan_leaf: debug
    homeassistant.components.switch.nissan_leaf: debug
```
