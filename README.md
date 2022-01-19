# AI-Deck ESP32 firmware

This firmware is running on the ESP32 Nina W102 module of the AI-Deck. It
implements communication with the GAP8 chip, the Crazyflie's STM32 main MCU as
well as implementing Wifi communication.

**NOTE**: This firmware is still under heavy development and will probably change radically before
it's ready for public use.

**NOTE**: The default configuration now uses the NINA_SYSBOOT as log UART TX.

## Compile and flash

The firmware is currently designed to be flashed and run from the serial port.

It has been developed using the version 4.3.1 of the [Esp32 IDF].

To flash and run:
```
$ source ~/path/to/idf/export.sh
$ idf.py build
$ idf.py flash
$ idf.py monitor
```

## Using the JTAG Adapter

### Initial Set-up

```
git clone --recursive https://github.com/espressif/openocd-esp32.git
cd openocd-esp32
./bootstrap
./configure
make
```
### Flashing

```
../openocd-esp32/bin/openocd -f interface/ftdi/olimex-arm-usb-tiny-h.cfg -f board/esp-wroom-32.cfg -c "program_esp32 build/partition_table/partition-table.bin 0x8000 verify" -c "program_esp32 build/bootloader/bootloader.bin 0x1000 verify" -c "program_esp32 build/aideck_esp.bin 0x10000 verify reset exit"
```

[Esp32 IDF]: https://github.com/espressif/esp-idf.git
