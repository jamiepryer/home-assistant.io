---
layout: page
title: "World Tides"
description: "Instructions on how to add Tides information to Home Assistant."
date: 2017-08-23 08:00
sidebar: true
comments: false
sharing: true
footer: true
logo: worldtidesinfo.png
ha_category:
  - Environment
ha_release: 0.52
redirect_from:
 - /components/sensor.worldtidesinfo/
---

The `worldtidesinfo` sensor platform uses details from [World Tides](https://www.worldtides.info/) to provide information about the prediction for the tides for any location in the world.

## {% linkable_title Setup %}

Get your API key from your account at [https://www.worldtides.info/](https://www.worldtides.info/).

Note: You only get 100 credits per month, so you must set your scan interval to be daily.
This is measured in seconds and shown in the example below.

The only other issue to note, is that each scan will happen on every reboot, so if you are doing a lot of reboots as you build your system, you might want to distable this until you are ready and comment out the code. 

## {% linkable_title Configuration %}

To use this sensor, add the following to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry with a template to give you a simple readable format

sensor
 - platform: worldtidesinfo
    api_key: YOUR_API_KEY
    name: My Favourite Beach
    scan_interval: 43200 #this is once per day.
    latitude: !secret HomeLatitude 
    longitude: !secret HomeLongitude
  - platform: template
    sensors:
      tide_my_favourite_beach_next_high:
        value_template: '{{ as_timestamp(states.sensor.my_favourite_beach.attributes.high_tide_time_utc) | timestamp_custom("%a %d/%m/%Y %H:%M") }}'
        friendly_name: "My Favourite Beach Next High Tide"
      tide_my_favourite_beach_next_low:
        value_template: '{{ as_timestamp(states.sensor.my_favourite_beach.attributes.low_tide_time_utc) | timestamp_custom("%a %d/%m/%Y %H:%M") }}'
        friendly_name: "My Favourite Beach Next Low Tide"
```

{% configuration %}
api_key:
  description: Your API key.
  required: true
  type: string
name:
  description: Name to use in the frontend.
  required: false
  type: string
  default: WorldTidesInfo
scan_interval:
  description: how often the API is called in seconds. Recommend you change this to be daily, as per the example.
  required: false
  default: 3600 
latitude:
  description: Latitude of the location to display the tides.
  required: false
  type: float
  default: "The latitude in your `configuration.yaml` file."
longitude:
  description: Longitude of the location to display the tides.
  required: false
  type: float
  default: "The longitude in your `configuration.yaml` file."
{% endconfiguration %}
