---
layout: page
title: "Flux Led/MagicLight"
description: "Instructions on how to setup Flux led/MagicLight within Home Assistant."
date: 2015-07-17 20:09
sidebar: true
comments: false
sharing: true
footer: true
logo: magic_light.png
ha_category: Light
ha_iot_class: Local Polling
ha_release: 0.25
---

The `flux_led` support is integrated into Home Assistant as a light platform. Several brands of both bulbs and controllers use the same protocol and they have the HF-LPB100 chipset in common. The chances are high that your bulb or controller (eg. WiFi LED CONTROLLER) will work if you can control the device with the MagicHome app.

Example of bulbs:

- [Flux Smart Lighting](http://www.fluxsmartlighting.com/)
- [MagicLight® Plus - WiFi Smart LED Light Bulb4](https://www.amazon.com/gp/product/B00NOC93NG)
- [Flux WiFi Smart LED Light Bulb4](http://smile.amazon.com/Flux-WiFi-Smart-Light-Bulb/dp/B01A6GHHTE)
- [WIFI smart LED light Bulb1](http://smile.amazon.com/gp/product/B01CS1EZYK)

Examples of controllers:

- [Ledenet WiFi RGBW Controller](https://www.amazon.com/gp/product/B01DY56N8U)
- [SUPERNIGHT WiFi Wireless LED Smart Controller](https://www.amazon.com/dp/B01JZ2SI6Q)


### {% linkable_title Configuration Details %}


To enable those lights, add the following lines to your `configuration.yaml` file:

```yaml
# Example configuration.yaml entry
light:
  - platform: flux_led
```

{% configuration %}
automatic_add:
  description: To enable the automatic addition of lights on startup.
  required: false
  default: false
  type: boolean
devices:
  description: A list of devices with their ip address.
  required: false
  type: list
  keys:
    name:
      description: A friendly name for the device.
      required: false
      type: string
    mode:
      description: "The chosen brightness mode, options are: `rgbw`, `rgb` and `w`."
      required: false
      default: rgbw
      type: string
    protocol:
      description: Set this to `ledenet` if you are using a ledenet bulb.
      required: false
      type: string
    custom_effect:
      description: A definition of the custom effect.
      required: false
      type: map
      keys:
        colors:
          description: A list of 1 to 16 colors, used in the effect loop (see example below). Defined as three comma-separated integers between 0 and 255 that represent the color in RGB. There is no way to set brightness, but you can define lower RGB values to simulate lower brightness. E.g., if you want 50% red, define it as `[127,0,0]` instead of `[255,0,0]`.
          required: true
          type: list
        speed_pct:
          description: A speed in percents (100 being the fastest), at which controller will transition between the colors.
          required: false
          type: integer
          default: 50
        transition:
          description: "A type of transition, which will be used to transition between the colors. Supported values are: `gradual`, `jump` and `strobe`."
          required: false
          type: string
          default: gradual
{% endconfiguration %}

<p class='note'>
Depending on your controller or bulb type, there are two ways to configure brightness.
The component defaults to rgbw. If your device has a separate white channel, you do not need to specify anything else; changing the white value will adjust the brightness of white channel keeping rgb color constant. However, if your device does not have a separate white channel, you will need to set the mode to rgb. In this mode, the device will keep the same color, and adjust the rgb values to dim or brighten the color.
</p>


### {% linkable_title Example configuration %}

Will automatically search and add all lights on start up:

```yaml
# Example configuration.yaml entry
light:
  - platform: flux_led
    automatic_add: true
```

Will add two lights with given name and create an automation rule to randomly set color each 45 seconds:

```yaml
light:
# Example configuration.yaml entry
  - platform: flux_led
    devices:
      192.168.0.106:
        name: flux_lamppost
      192.168.0.109:
        name: flux_living_room_lamp

automation:
  alias: random_flux_living_room_lamp
  trigger:
    platform: time_pattern
    seconds: '/45'
  action:
    service: light.turn_on
    data:
      entity_id: light.flux_living_room_lamp
      effect: random
```

Will add a light without the white mode:

```yaml
    192.168.1.10:
      name: NAME
      mode: "rgb"
```

Will add a light with rgb+white mode (default). White and RGB channels can be adjusted independently using a slider and color picker respectively.

```yaml
    192.168.1.10:
      name: NAME
      mode: "rgbw"
```

Will add a light with white mode only. This is useful when only W channel is connected to an RGBW controller and allows the white level to be controlled via brightness value.

```yaml
    192.168.1.10:
      name: NAME
      mode: "w"
```

Some devices such as the Ledenet RGBW controller use a slightly different protocol for communicating the brightness to each color channel. If your device is only turning on or off but not changing color or brightness try adding the LEDENET protocol.

```yaml
light:
  - platform: flux_led
    devices:
      192.168.1.10:
        name: NAME
        protocol: 'ledenet'
```

### {% linkable_title Effects %}

The Flux Led light offers a number of effects which are not included in other lighting packages. These can be selected from the front-end, or sent in the effect field of the `light.turn_on` command.

| Effect Name                                                                                                  | Description                                                        |
|--------------------------------------------------------------------------------------------------------------|--------------------------------------------------------------------|
| `colorloop`                                                                                                  | Smoothly transitions through the rainbow.                          |
| `colorjump`                                                                                                  | Jumps through seven different rainbow colors.                      |
| `colorstrobe`                                                                                                | Strobes each rainbow color in a loop.                              |
| `red_fade`, `green_fade`, `blue_fade`, `yellow_fade`, `cyan_fade`, `purple_fade`, `white_fade`               | Fades between the color as indicated in the effect name and black. |
| `rg_cross_fade`                                                                                              | Fades between red and green.                                       |
| `rb_cross_fade`                                                                                              | Fades between red and blue.                                        |
| `gb_cross_fade`                                                                                              | Fades between green and blue.                                      |
| `red_strobe`, `green_strobe`, `blue_strobe`, `yellow_strobe`, `cyan_strobe`, `purple_strobe`, `white_strobe` | Strobes the color indicated by the effect name.                    |
| `random`                                                                                                     | Chooses a random color by selecting random values for R, G, and B. |
| `custom`                                                                                                     | Custom effect (if defined, see below).                             |

Users can define their own custom effect. It consists of three parameters: a list of 1 to 16 colors, speed and type of transition. The controller will transition between the colors in a loop, with sepcified transition and speed. Here is an example of a custom effect that will quickly flash red, yellow, green, cyan, blue, magenta in a loop:

```yaml
light:
  - platform: flux_led
    devices:
      192.168.1.10:
        custom_effect:
          speed_pct: 100
          transition: 'strobe'
          colors:
            - [255,0,0]
            - [255,255,0]
            - [0,255,0]
            - [0,255,255]
            - [0,0,255]
            - [255,0,255]
```
