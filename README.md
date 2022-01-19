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

## Using the JTAG Adapter (Docker)

### Initial Set-up

```
docker pull bitcraze/aideck-nina
```

### Install IDF

```
git clone https://github.com/espressif/esp-idf.git
cd esp-idf
git checkout v4.3.1
git submodule sync
git submodule update --init --recursive
./install.sh
```

### Building and Flashing

```
source ../esp-idf/export.sh
idf.py app bootloader
docker run --rm -it -v $PWD:/module/ --device /dev/ttyUSB0 --privileged -P bitcraze/aideck-nina /bin/bash -c "/openocd-esp32/bin/openocd -f interface/ftdi/olimex-arm-usb-tiny-h.cfg -f board/esp-wroom-32.cfg -c 'program_esp32 build/bootloader/bootloader.bin 0x1000 verify' -c 'program_esp32 build/aideck_esp.bin 0x10000 verify reset exit'"
```

## Using the JTAG Adapter (Local Install; Untested!)

### Initial Set-up

```
git clone --recursive https://github.com/espressif/openocd-esp32.git
cd openocd-esp32
./bootstrap
./configure
make
sudo make install
```

### Flashing

```
../openocd-esp32/bin/openocd -f interface/ftdi/olimex-arm-usb-tiny-h.cfg -f board/esp-wroom-32.cfg -c "program_esp32 build/partition_table/partition-table.bin 0x8000 verify" -c "program_esp32 build/bootloader/bootloader.bin 0x1000 verify" -c "program_esp32 build/aideck_esp.bin 0x10000 verify reset exit"
```

[Esp32 IDF]: https://github.com/espressif/esp-idf.git
