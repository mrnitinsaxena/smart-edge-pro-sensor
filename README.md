<p>
  <a href="https://resonatetoinnovate.com/"><img src="static/images/resonate-to-innovate-logo.svg" alt="Resonate to Innovate" width="190" align="left"></a>
  <a href="https://resonatetoinnovate.com/"><img src="static/images/smart-edge-logo.svg" alt="Smart Edge" width="300" align="right"></a>
</p>
<br clear="both">

<h1 align="center">Pro Sensor</h1>

> [!IMPORTANT]
> **This repository has moved to [github.com/mrnitinsaxena/smart-edge-pro-sensor](https://github.com/mrnitinsaxena/smart-edge-pro-sensor).** Sensors update to the new location automatically with v2026.11.25. New releases are published only there.

<p align="center">
  <a href="https://mrnitinsaxena.github.io/smart-edge-pro-sensor/"><b>Install firmware</b></a> ·
  <a href="https://github.com/mrnitinsaxena/smart-edge-pro-sensor/releases">Releases</a>
</p>

ESPHome firmware for the **Smart Edge Pro Sensor** by [Resonate to Innovate](https://resonatetoinnovate.com/): an ESP32-based room sensor with temperature, humidity and light sensing, optional mmWave presence detection, IR remote control and a status LED with a buzzer. Built for [Home Assistant](https://www.home-assistant.io/).

## Features

- **Environment:** temperature and humidity (AHT20) and illuminance (BH1750), each with an adjustable calibration offset
- **Presence (optional):** works with the HLK-LD2410, LD2412, LD2420 and LD2450, DFRobot SEN0395 and SEN0609, and Seeed MR24HPC1 mmWave radars; dual-radar models add an HLK-LD2450 as a second radar
- **IR control (optional):** Samsung and Sony TVs, Daikin and Panasonic ACs, Atomberg fans
- **Alerts:** a high-temperature alert with an adjustable threshold, a siren and an emergency LED pattern; follows Alarmo automatically, and an **External alarm** switch lets any Home Assistant automation sound the siren
- **Status feedback:** RGB LED and buzzer for boot, Wi-Fi and Home Assistant connection, with a Do Not Disturb switch
- **Easy setup:** Wi-Fi through the sensor's own hotspot (works on any phone); adopt in Home Assistant and the ESPHome dashboard
- **Secure by default:** no shared passwords or keys; Home Assistant sets the API encryption key when it adopts the device

## Models

Every release is built for each model. A sensor only ever receives updates for its own model.

| Model | Radar (UART) | Firmware file |
|---|---|---|
| Pro Sensor | none | `pro_sensor/pro-sensor.yaml` |
| Pro Sensor + SEN0395 | DFRobot SEN0395 | `pro_sensor/pro-sensor-sen0395.yaml` |
| Pro Sensor + SEN0609 | DFRobot SEN0609 | `pro_sensor/pro-sensor-sen0609.yaml` |
| Pro Sensor + MR24HPC1 | Seeed MR24HPC1 | `pro_sensor/pro-sensor-mr24hpc1.yaml` |
| Pro Sensor + LD2410 | HLK-LD2410 | `pro_sensor/pro-sensor-ld2410.yaml` |
| Pro Sensor + LD2412 | HLK-LD2412 | `pro_sensor/pro-sensor-ld2412.yaml` |
| Pro Sensor + LD2420 | HLK-LD2420 | `pro_sensor/pro-sensor-ld2420.yaml` |
| Pro Sensor + LD2450 | HLK-LD2450 | `pro_sensor/pro-sensor-ld2450.yaml` |

**Dual radar** (radar 2 = HLK-LD2450 on GPIO 26/27; LD2450 can't be radar 1 and radar 2 at the same time):

| Model | Radar 1 | Radar 2 | Firmware file |
|---|---|---|---|
| Pro Sensor + SEN0395 + LD2450 | DFRobot SEN0395 | HLK-LD2450 | `pro_sensor/pro-sensor-sen0395-ld2450.yaml` |
| Pro Sensor + SEN0609 + LD2450 | DFRobot SEN0609 | HLK-LD2450 | `pro_sensor/pro-sensor-sen0609-ld2450.yaml` |
| Pro Sensor + MR24HPC1 + LD2450 | Seeed MR24HPC1 | HLK-LD2450 | `pro_sensor/pro-sensor-mr24hpc1-ld2450.yaml` |
| Pro Sensor + LD2410 + LD2450 | HLK-LD2410 | HLK-LD2450 | `pro_sensor/pro-sensor-ld2410-ld2450.yaml` |
| Pro Sensor + LD2412 + LD2450 | HLK-LD2412 | HLK-LD2450 | `pro_sensor/pro-sensor-ld2412-ld2450.yaml` |
| Pro Sensor + LD2420 + LD2450 | HLK-LD2420 | HLK-LD2450 | `pro_sensor/pro-sensor-ld2420-ld2450.yaml` |

Radar 2 adds its own entities (**Presence 2**, **Motion 2**, and *Radar 2 …* settings); radar 1 keeps its names.

All models include every IR remote brand (picked in Home Assistant) and update automatically from Home Assistant.

## Repository layout

```
pro_sensor/
├── pro-sensor*.yaml      # one file per model - the firmware the release workflow builds
├── hardware/             # pins per board revision (hw-2025.08.19.yaml)
├── packages/             # shared code: core.yaml, radar (UART) and IR remote packages
├── components/panaac/    # Panasonic AC driver
└── examples/             # all-options.example.yaml - every setting with its default
static/                   # web installer page and logos
.github/workflows/        # builds every model on each release
```

## Getting started

1. **Install the firmware** (skip if your sensor came pre-installed): open the [web installer](https://mrnitinsaxena.github.io/smart-edge-pro-sensor/) in Chrome or Edge, **choose the model that matches your radar**, connect the sensor by USB and click **Install firmware**.
2. **Power the device** from a 5 V USB supply. It beeps once and the LED pulses green.
3. **Connect it to Wi-Fi:** on your phone, join the `pro-sensor-xxxxxx` Wi-Fi network. The setup page opens automatically (or go to `192.168.4.1`); pick your network and enter the password.

   Two beeps and a blue LED mean it's online.
4. **Add it to Home Assistant.** It's discovered automatically under *Settings → Devices & services*. Click **Add**. Three beeps mean Home Assistant has connected.

### Customize the firmware (optional)

In the ESPHome dashboard the device shows up as **Discovered**. Click **Adopt** to take over its configuration, then add any of the options below.

You can also start from scratch:

```yaml
packages:
  smart_edge: github://mrnitinsaxena/smart-edge-pro-sensor/pro_sensor/pro-sensor-sen0395.yaml@v2026.11.25

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password
```

## Configuration options

Set any of these under `substitutions:` in your device config.

### Presence sensors (enable one radar 1; optionally radar 2; all use UART)

| Option | Sensor |
|---|---|
| `radar1_ld2410: true` | HLK-LD2410 |
| `radar1_ld2412: true` | HLK-LD2412 |
| `radar1_ld2420: true` | HLK-LD2420 |
| `radar1_ld2450: true` | HLK-LD2450 |
| `radar1_sen0395: true` | DFRobot SEN0395 |
| `radar1_sen0609: true` | DFRobot SEN0609 |
| `radar1_mr24hpc1: true` | Seeed MR24HPC1 |
| `radar2_ld2450: true` | HLK-LD2450 as radar 2 (dual-radar boards, GPIO 26/27; not with `radar1_ld2450`) |

### IR remotes (all enabled by default)

| Option | Device |
|---|---|
| `ir_samsung_tv: true` | Samsung TV |
| `ir_sony_tv: true` | Sony TV |
| `ir_daikin_ac: true` | Daikin AC |
| `ir_panasonic_ac: true` | Panasonic AC |
| `ir_atomberg_fan: true` | Atomberg fan |

### Device and network

| Option | Default | Description |
|---|---|---|
| `custom_device_name` | `pro-sensor-xxxxxx` | Hostname |
| `custom_friendly_name` | `Pro Sensor` | Name shown in Home Assistant |
| `custom_area` | — | Room suggested to Home Assistant when the device is first added (e.g. `Guest Room`). Keep the room out of the name to avoid IDs like `guest_room_guest_room_sensor_…` |
| `wifi_ssid` / `wifi_password` | — | Hard-code Wi-Fi instead of using onboarding |
| `api_key` | provisioned by HA | Fix the API encryption key (also encrypts OTA) |
| `web_username` / `web_password` | off | Enable the built-in web page (both required) |
| `wifi_power_mode` | `NONE` | `NONE`, `LIGHT` or `HIGH` power saving |
| `cpu_frequency_mode` | `LIGHT` (160 MHz) | `NONE` = 240 MHz; any other value = 80 MHz |
| `logger_level` | `WARN` | ESPHome log level |
| `usb_logs` | `false` | Logs over the USB cable (115200 baud) |

### Alerts and LEDs

| Option | Default | Description |
|---|---|---|
| `alarm_panel_entity` | `alarm_control_panel.alarmo` | Home Assistant alarm panel that triggers the siren when it goes off |
| `alarm_panel` | `true` | Set `false` to stop following an alarm panel |

**LEDs.** The on-board LED always shows the status signals. You can also connect a WS2812 strip (up to 60 LEDs) to the **LED connector (GPIO 33)**. It's a separate light that you control yourself. In Home Assistant, under the device's *Configuration*:

- **External LED strip:** turn on to use a strip. The device restarts, and an **LED strip** light appears.
- **Strip length:** the number of LEDs on your strip (1–60).
- **Power supply:** match your adapter. Strip brightness is capped to stay within that budget.

During an alert, the strip (if enabled) flashes the same red/blue emergency pattern as the on-board LED, then returns to what it was doing before.

Strips longer than a few LEDs need their own 5 V supply; connect its ground to the sensor's ground.

## Status signals

| Event | LED | Buzzer |
|---|---|---|
| Booted | Green pulse | 1 beep |
| Wi-Fi connected | Blue pulse | 2 beeps |
| Home Assistant connected | Off | 3 beeps |
| Wi-Fi / HA lost | Fast pulse | Long tone |
| Alert | Red/blue flashing | Siren |
| Firmware update available | Slow yellow pulse, 10 s | – |
| Updating / done / failed | Purple / green / red | – |

**Do Not Disturb** (off by default) silences the connection sounds and LEDs, and the update-available pulse. Alerts always sound.

## Alarms

- **Alarmo:** if you use [Alarmo](https://github.com/nielsfaber/alarmo), the sensor sounds its siren automatically whenever the alarm is triggered.
- **Anything else:** use the **External alarm** switch. For example, create an automation: *when the smoke detector or water leak sensor turns on, turn on Pro Sensor External alarm*. Turn the switch off to silence it.

## Choosing your IR remotes

Under the device's **Configuration** in Home Assistant, set **TV remote**, **AC remote** and **Fan remote** to your brands. The device restarts and only shows the controls for the brands you chose. If you switch brands later, update any automations that used the old controls.

## Factory reset

- **Without Home Assistant:** unplug and replug the power **5 times**, with each replug within 10 seconds. The device erases its Wi-Fi and API key and restarts in setup mode.
- **From Home Assistant:** turn on the **Factory reset** switch under the device's configuration entities.

## Hardware

Board revision **2025.08.19** (shown as *Hardware version* in Home Assistant). This firmware supports this board only.

| Function | GPIO |
|---|---|
| I²C SDA / SCL (AHT20, BH1750) | 21 / 22 |
| Buzzer | 23 |
| WS2812 status LED (on-board) | 25 |
| External WS2812 LED strip | 33 |
| IR receiver | 18 |
| IR transmitter | 19 |
| Radar 1 UART RX / TX | 16 / 17 (SEN0609: 17 / 16) |
| Radar 2 UART RX / TX (LD2450, dual-radar models) | 26 / 27 |

MCU: ESP32 (rev 3.1+), ESP-IDF framework.

Each board revision has its own pin file in `pro_sensor/hardware/` and its own set of model files and update channel, so a device only ever receives firmware built for its own board.

## Good to know

- **Time zone:** taken automatically from your Home Assistant (2026.3 or newer).
- **Radar first start (LD2410, LD2412, LD2450):** the radar's own Bluetooth is switched off, and a presence delay of 0 is set to 30 s. This happens once; you can change both afterwards.
- **Alerts and Wi-Fi:** the device normally restarts after 15 minutes without Wi-Fi. During an alert it waits, so the siren keeps sounding.
- **Sensor updates:** temperature, humidity and light are reported when they change (±0.2 °C, ±1 %, ±10 %) or at least every 5 minutes, keeping your Home Assistant database small.
- **Diagnostics:** CPU temperature, uptime and the Home Assistant connection sensor are hidden by default; enable them under the device's entities if you need them.
- **Security:** like most ESPHome devices, the sensor accepts firmware updates from your local network. Keep it on a trusted network (or an IoT VLAN), and don't expose it to the internet.

## ⚠️ Safety notice

The high-temperature alert is a convenience feature. It is **not** a certified smoke or fire alarm and must not replace one. Always install certified smoke detectors.

## License

MIT. See [LICENSE](LICENSE).

---

<p align="center">
  Designed and built by<br><br>
  <a href="https://resonatetoinnovate.com/">
    <picture>
      <img src="static/images/resonate-to-innovate-logo.svg" alt="Resonate to Innovate" width="240">
    </picture>
  </a>
  <br>
  <a href="https://resonatetoinnovate.com/">resonatetoinnovate.com</a>
</p>
