---
layout: page
title: "Reddit Sensor"
description: "How to integrate the Reddit sensor into Home Assistant."
date: 2018-02-21 20:00
sidebar: true
comments: false
sharing: true
footer: true
logo: reddit.png
ha_category: Sensor
ha_release: 0.89
ha_iot_class: Cloud Polling
---

The Reddit sensor integrates data from [Reddit](https://reddit.com/) to monitor your favorite subreddits.

## {% linkable_title Setup %}

To set up this sensor, you will need to generate a `client_id` and `client_secret` for the user account you will use to connect. Follow the first steps in [this wiki](https://github.com/reddit-archive/reddit/wiki/OAuth2-Quick-Start-Example).

## {% linkable_title Configuration %}

To enable this platform, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
- platform: reddit
  username: !secret reddit_username
  password: !secret reddit_password
  client_id: !secret reddit_client_id
  client_secret: !secret reddit_client_secret
  subreddits:
    - news
    - worldnews
```

{% configuration %}
username:
  description: Your Reddit account username.
  required: true
  type: string
password:
  description: Your Reddit account password.
  required: true
  type: string
client_id:
  description: Your Reddit account client_id generated from the wiki above.
  required: true
  type: string
client_secret:
  description: Your Reddit account client_secret generated from the wiki above.
  required: true
  type: string
subreddits:
  description: List of subreddits you want to get data on.
  required: true
  type: list
sort_by:
  description: Sort reddit posts by new, top, controversial, and hot.
  required: false
  type: string
  default: hot
{% endconfiguration %}
