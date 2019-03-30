---
layout: page
title: "Spider"
description: "Instructions on how to setup the Spider hub within Home Assistant."
date: 2018-07-17 22:01
sidebar: true
comments: false
sharing: true
footer: true
logo: spider.png
ha_category:
  - Hub
  - Climate
  - Switch
ha_iot_class: Cloud Polling
ha_release: 0.75
redirect_from:
  - /components/switch.spider/
  - /components/climate.spider/
---

The `spider` component is the main component to integrate all [Itho Daalderop Spider](https://www.ithodaalderop.nl/spider-thermostaat) related platforms. You will need your Spider account information (username, password) to discover and control devices which are related to your account.

There is currently support for the following device types within Home Assistant:

- Climate
- Switch

## {% linkable_title Configuration %}

To add your Spider devices into your Home Assistant installation, add the following to your `configuration.yaml` file:

```yaml
spider:
  username: YOUR_USERNAME
  password: YOUR_PASSWORD
```

{% configuration %}
username:
  description: Account username of mijn.ithodaalderop.nl
  required: true
  type: string
password:
  description: Account password of mijn.ithodaalderop.nl
  required: true
  type: string
scan_interval:
  description: How frequently to query for new data. Defaults to 120 seconds.
  required: false
  type: integer
{% endconfiguration %}

<p class='note warning'>
This component is not affiliated with Itho Daalderop Spider and retrieves data from the endpoints of the mobile application. Use at your own risk.
</p>

### {% linkable_title Climate %}

<p class='note'>
Although this component lets you change the operation mode to heating or cooling, it doesn't necessarily mean your boiler can. Spider is not aware of your current situation.
</p>
