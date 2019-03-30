---
layout: page
title: "Sony PlayStation 4"
description: "Instructions on how to integrate a Sony PlayStation 4 into Home Assistant."
date: 2019-02-12 01:08
sidebar: true
comments: false
sharing: true
footer: true
logo: ps4.png
ha_category: Media Player
ha_release: 0.89
ha_iot_class: Local Polling
---

The `ps4` component allows you to control a
[Sony PlayStation 4 console](https://www.playstation.com/en-us/explore/ps4/).

- This component supports controlling a single PlayStation 4 for your instance. Additional consoles may be supported in a future release.

## {% linkable_title Requirements %}

- Android or iOS device
- PS4 Second Screen App for [Android](https://play.google.com/store/apps/details?id=com.playstation.mobile2ndscreen&hl=en_US) or [iOS](https://itunes.apple.com/us/app/ps4-second-screen/id1201372796?mt=8) installed on device.

## {% linkable_title Set up %}

1. Download the Second Screen App and make sure that you can find and control your PlayStation 4 normally.

<p class='note'>
  Read the section "Granting Port Access" below before continuing.
</p>

2. Navigate to `Configuration -> Integrations` and select `Configure` for `PlayStation 4`.

3. Follow instructions displayed to generate user credentials. You will know this step is completed when a form with fields appears.

4. Pair Home Assistant to your PlayStation 4 by filling in the fields.
- **Note:** To find your correct region refer to the section [Regions](#regions)

## {% linkable_title Granting Port Access %}

The PlayStation 4 component requires the use of privileged ports to work correctly, specifically UDP port 987 and TCP port 997. Depending on your OS of your Home Assistant instance you may need to allow usage of privileged ports manually.

<p class='note warning'>
  Do not run your <b>Home Assistant</b> instance itself as <b>root</b> or with <b>root/sudo privileges</b> to accomplish this. This would create a security risk for your host system.
</p>

There are varying methods to perform this, dependent on your OS that is running Home Assistant. Specifically, your *Python Interpreter* which runs your Home Assistant instance needs access to the mentioned ports.

<p class='note'>
  If your Home Assistant device is running <b>Hass.io</b> on <b>HassOS</b>, it does not require additional configuration.
</p>

### {% linkable_title Debian-based %}
Home Assistant installed on a Debian-type OS may require configuration. This section is applicable but not limited to the following operating systems:

- Debian
- Hassbian
- Rassbian
- Armbian
- Ubuntu

In terminal run the following command:

```bash
sudo setcap 'cap_net_bind_service=+ep' <python>
```

Replace `<python>` with your **system path** to Python that is running Home Assistant and/or your virtual environment if used. The path **should not** be a **symlink** or be **inside of a virtual environment**.

Example:
```bash
sudo setcap 'cap_net_bind_service=+ep' /usr/bin/python3.5
```

To find your system Python path:

- Add the [System Health](https://www.home-assistant.io/components/system_health/) component to your `configuration.yaml`. In a web browser access your frontend and navigate to the about/logs page "http://<yourhomeassistanturl>/dev-info). In the System Health box locate the item **python_version** and note the value that is displayed. Then in terminal run:

  ```bash
  whereis python<version>
  ```

  Replace `<version>` with the value for `python_version` that is shown in the System Health box.

  Example:
  ```bash
  whereis python3.5.3
  ```

  The output which has the directory `/bin/` is likely your system python path which should look like this `/usr/bin/python3.5`

- If Home Assistant is installed in a virtual environment, use terminal to `cd` to the root/top directory of your environment and run:

  ```bash
  readlink -f bin/python3
  ```
  or
  ```bash
  readlink -f bin/python
  ```

  The output will be your system Python path.

### {% linkable_title Docker %}

When running Home Assistant using Docker, make sure that the Home Assistant container is discoverable by the PS4. This can be achieved by ensuring that the Home Assistant container uses the `host` network driver (by passing `--net=host` to the container when creating, or adding `network_mode: "host"` to your compose file when using `docker-compose`).

## {% linkable_title Configuration %}

<p class='note'>
  The PlayStation 4 component does not use entries from `configuration.yaml`. You must configure this component by using `Integrations`
</p>

## {% linkable_title Regions %}

Some titles will have different SKUs in the PlayStation Store database depending on your region. You must select your specific region in the setup in order to retrieve the cover art for such titles correctly. The component will attempt to search other databases for the correct title if it cannot be found, although it will take longer to do so and may fetch an incorrect cover.

|  Available Regions                                                          | Unavailable Regions        |
| --------------------------------------------------------------------------- | -------------------------- |
| Argentina, Australia, Austria, Bahrain, Belgium, Brazil, Bulgaria,          | China, Japan, Phillipines, |
| Canada, Chile, Columbia, Costa Rica, Croatia, Cyprus, Czech Republic,       | Serbia, Ukraine, Vietnam   |
| Denmark, Ecuador, El Salvador, Finland, France, Germany, Greece, Guatemala, |                            |
| Honduras, Hong Kong, Hungary, Iceland, India, Indonesia, Ireland, Israel,   |                            |
| Italy, Korea, Kuwait, Lebanon, Luxembourg, Malta, Maylasia, Mexico,         |                            |
| Middle East, Nederland, New Zealand, Nicaragua, Norway, Oman, Panama,       |                            |
| Peru, Poland, Portugal, Qatar, Romania, Russia, Saudi Arabia, Singapore,    |                            |
| Slovakia, Slovenia, South Africa, Spain, Sweden, Switzerland, Taiwan,       |                            |
| Thailand, Turkey, United Arab Emirates, United Kingdom, United States       |                            |


<p class='note'>
  The regions which are unavailable have no database or have formatting in the database which can not be used by the component.
</p>

## {% linkable_title Services %}

### {% linkable_title Service `send_command` %}

Emulate button press on PlayStation 4. This emulates the commands available for the PS4 Second Screen App. This is not to be confused with DualShock 4 controller buttons.

| Service data attribute | Optional | Example                      | Description                           |
| ---------------------- | -------- | ---------------------------- | ------------------------------------- |
| `entity_id`            | No       | `media_player.playstation_4` | The entity id for your PlayStation 4. |
| `command`              | No       | `ps`                         | The command you want to send.         |

#### {% linkable_title Available Commands %}

Full list of supported commands.

| Command  | Button Emulated  |
| -------- | ---------------- |
| `ps`     | PS (PlayStation) |
| `option` | Option           |
| `enter`  | Enter            |
| `back`   | Back             |
| `up`     | Swipe Up         |
| `down`   | Swipe Down       |
| `left`   | Swipe Left       |
| `right`  | Swipe Right      |

## {% linkable_title Troubleshooting %}

### {% linkable_title Cover Art Issues %}
If you are running a game/title on your PS4 that does not display a cover or displays the incorrect cover, post an issue [here](https://github.com/ktnrg45/pyps4-homeassistant/issues).

Be sure to include the following information:
- Your Country

As well as the exact values for the following attributes found in the state of your PS4 entity.
- media_title
- media_content_id
