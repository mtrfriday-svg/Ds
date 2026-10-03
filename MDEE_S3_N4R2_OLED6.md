# M'DEE ESP32-S3 N4R2 + 0.96in OLED + 6 buttons

Custom build target based on ESP32 Marauder source.

## Board
- ESP32-S3 N4R2
- 4 MB flash
- 2 MB QSPI PSRAM
- Arduino-ESP32 2.0.11

## OLED
0.96 inch SSD1306 128x64 I2C, address 0x3C. Driven directly via the standalone
`mdee_oled` object in `esp32_marauder.ino` (not through the Display.cpp/TFT_eSPI
menu system — `HAS_SCREEN` is intentionally left undefined for this board).
- SDA GPIO 8
- SCL GPIO 9

## Buttons
All buttons are active LOW and connect between the GPIO and GND:
- UP GPIO 1
- DOWN GPIO 10
- LEFT GPIO 11
- RIGHT GPIO 12
- SELECT GPIO 13
- BACK/STATUS GPIO 14

The sixth button (`B_BTN`) is read directly in `esp32_marauder.ino` as the
M'DEE local status button; it isn't part of the upstream menu-navigation set
(`HAS_L`/`HAS_R`/`HAS_U`/`HAS_D`/`HAS_C`), since those only matter when
`HAS_SCREEN` is enabled.

## Board macro
`MDEE_S3_SUPER_MINI` is defined directly in `esp32_marauder/configs.h` (under
`//// BOARD TARGETS`) as its own standalone board — it does not alias
`MARAUDER_MINI`. That board is tied to the original ESP32 (idf 3.3.4, NimBLE
2.x, `esp32:esp32:d32` FQBN) with a TFT_eSPI SPI screen, SD, GPS and a temp
sensor, none of which match this ESP32-S3 + I2C-OLED hardware.

## GitHub Actions
Push the repository to GitHub. The workflow `.github/workflows/build_mdee.yml`
builds `esp32_marauder_<version>_<date>_mdee_s3_n4r2_oled6.bin` against
`esp32:esp32:esp32s3:PartitionScheme=min_spiffs,FlashSize=4M,PSRAM=enabled`
(Arduino-ESP32 2.0.11). The binary is available under the Actions run's
Artifacts section.
