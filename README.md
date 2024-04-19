This repository contains a Display component for [ESPHome](https://esphome.io/)
to support the ESP32-S3 [LILYGO T5 4.7" Plus E-paper display](https://www.lilygo.cc/products/t5-4-7-inch-e-paper-v2-3).

(Do not confuse it with the original ESP32-based Lilygo T5 4.7 board.)

For more info in the display components, see the [ESPHome display documentation](https://esphome.io/#display-components)

## Usage

To use the board with [ESPHome](https://esphome.io/) **you have to put quite a
number of options in your esphome config**:
* Configure the aprpopriate board, variant, and framework versions in the
[esp32 platform](https://esphome.io/components/esp32.html)
* Set a bunch of `platformio_options`
* Include the component from this repository as `external_components` 

If you clone this repository, a working example is included:

    git clone https://github.com/nickolay/esphome-lilygo-t547plus.git
    cd esphome-lilygo-t547plus
    esphome run basic.yaml

If you don't want to clone, copy the necessary pieces from [clock.yaml](./clock.yaml) [components](./components) folder  & [fonts](./fonts) folder
and adapt the `external_components` configuration as follows:

```yaml
esphome:
  name: lilygo
  platformio_options:
    # Unless noted otherwise, based on https://github.com/Xinyuan-LilyGO/LilyGo-EPD47/blob/1eb6119fc31fcff7a6bafecb09f4225313859fc5/examples/demo/platformio.ini#L37
    upload_speed: 921600
    monitor_speed: 115200
    board_build.mcu: esp32s3
    board_build.f_cpu: 240000000L
    board_build.arduino.memory_type: qspi_opi
    board_build.flash_size: 16MB
    board_build.flash_mode: qio
    board_build.flash_type: qspi
    board_build.psram_type: opi
    board_build.memory_type: qspi_opi
    board_build.boot_freq: 80m
    platform_packages:
      - "toolchain-riscv32-esp @8.4.0+2021r2-patch5"
    build_flags:  # the first three defines are required for the screen library to function.
      - "-DBOARD_HAS_PSRAM"
      - "-DARDUINO_RUNNING_CORE=0"  # TODO: this conflicts with the value from platformio's idedata, spewing a lot of warnings during the build.
      - "-DARDUINO_EVENT_RUNNING_CORE=0"  # and this too
      # In addition to lilygo's settings:
      # To enable reading logs over USB until `hardware_uart: USB_CDC` support
      # is added to `logger:`, as detailed in <https://github.com/esphome/feature-requests/issues/1906>:
      - "-DARDUINO_USB_MODE=1"
      - "-DARDUINO_USB_CDC_ON_BOOT=1"

esp32:
  variant: esp32s3
  board: esp32-s3-devkitc-1

  framework:
    type: arduino
    # Just like in <https://community.home-assistant.io/t/enable-usb-cdc-to-log-hello-world-to-esp32-s3-dev-board-for-esphome/463164/10>
    # I had problems with newer versions; the following combination happens to work, so using it for now.
    version: 2.0.3
    platform_version: 5.1.1

logger:
  level: VERBOSE
  # hardware_uart: USB_CDC  # see note about <https://github.com/esphome/feature-requests/issues/1906> above


# Enable Home Assistant API
api:
  encryption:
    key: "your_enc_key"

ota:
  password: "your_enc_password"

wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_password

  # Enable fallback hotspot (captive portal) in case wifi connection fails
  ap:
    ssid: "E-Paper Fallback Hotspot"
    password: "esphome123"

binary_sensor:
  - platform: gpio
    pin: 
      number: GPIO21 #was GPIO39 on the previous board
      inverted: true
    name: "Button 1"
    on_press:
      - logger.log: PhysButton Pressed

external_components:
  source:
    type: local
    path: components/  #/ create a folder named components under the esphome directory : esphome/components then make another inside the components folder named t547 : esphome/components/t547 and copy the contents of the git folder t547 into it. /#
  components: ["t547"]

captive_portal:
spi:
  clk_pin: GPIO3
  mosi_pin: GPIO2

font:
  - file: "fonts/OCRAEXT.ttf" #/ create a folder called fonts under the main esphome folder if you do not already have one and copy the font file OCRAEXT.ttf into it or you will error during compiling the code /#
    id: fontocra_large
    size: 300
  - file: "fonts/OCRAEXT.ttf"
    id: fontocra_small
    size: 200

time:
  - platform: homeassistant #/ make sure you have the Time & Date intergration installed to Home Assistant or you will not get the correct time set after the first screen refresh. /#
    id: local_time
    timezone: Europe/London

display:
  - platform: t547
    # rotation: 180
    update_interval: 60s
    lambda: |-
      it.strftime(20, 0, id(fontocra_large), TextAlign::TOP_LEFT, "%H:%M", id(local_time).now());
      it.line(0, 50, 960, 50);
      it.line(0, 270, 960, 270);
      it.strftime(0, 280, id(fontocra_small), TextAlign::TOP_LEFT, "%d-%m-%y", id(local_time).now());
      it.line(0, 500, 960, 500);
```

## Discussion

https://github.com/esphome/feature-requests/issues/1960
