This repository contains a Display component for [ESPHome](https://esphome.io/)
to support the ESP32-S3 [LILYGO T5 4.7" Plus E-paper display](https://www.lilygo.cc/products/t5-4-7-inch-e-paper-v2-3).

![lilygo t5 4 7 esp32 s3 ver2 3](https://github.com/CBDesignS/esphome-lilygo-t547plus/assets/54717994/234c47d1-d8d6-467a-abab-1def8c596f25)

(Do not confuse it with the original ESP32-based Lilygo T5 4.7 board.)

For more info in the display components, see the [ESPHome display documentation](https://esphome.io/#display-components)

## Usage

To use the board with [ESPHome](https://esphome.io/) **you have to put quite a
number of options in your esphome config**:
* Configure the aprpopriate board, variant, and framework versions in the
[esp32 platform](https://esphome.io/components/esp32.html)
* Set a bunch of `platformio_options`
* Include the component from this repository as `external_components` 

If you clone this repository, There are working examples included in the: [example_yaml](./example_yaml) folder

1. You will find A Basic working [basic.yaml](./example_yaml/basic.yaml), [components](./components) folder & [fonts](./fonts) folder

---

The examples below are for EspHome that read data from a working Home Assistant installation
1. You will find A Basic working [clock.yaml](./example_yaml/clock.yaml), required files are in th [components](./components) folder & [fonts](./fonts) folder
2. You will find my HA Weather Station [Weather.yaml](./example_yaml/Weather.yaml), required files are in the [components](./components) folder & [fonts](./fonts) folder & 3D STL Housing files [housing](./housing) folder

# Basic Clock. #
![Lilygo-t5-4 7s3](https://github.com/CBDesignS/esphome-lilygo-t547plus/assets/54717994/4ce49df6-4ba3-4dc1-8bbc-84d9ab0e87fe)

# Home Assistant Weather Station. #
![HA weather v2](https://github.com/CBDesignS/esphome-lilygo-t547plus/assets/54717994/dae11905-bf54-4ed1-8ff6-ddc6d275cd2f)


```

## Discussion

https://github.com/esphome/feature-requests/issues/1960
