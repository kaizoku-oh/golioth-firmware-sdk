# Golioth LightDB Set Sample

## Overview

This sample demonstrates how to connect to Golioth and set values inside
of LightDB.

## Requirements

* Golioth credentials
* Network connectivity

## Building and Running

Configure the following Kconfig options based on your Golioth
credentials:

* GOLIOTH_SAMPLE_PSK_ID - PSK ID of registered device
* GOLIOTH_SAMPLE_PSK - PSK of registered device

by adding these lines to configuration file (e.g. `prj.conf`):

```cfg
CONFIG_GOLIOTH_SAMPLE_PSK_ID="my-psk-id"
CONFIG_GOLIOTH_SAMPLE_PSK="my-psk"
```

### Platform specific configuration

#### QEMU

This application has been built and tested with QEMU x86 (qemu_x86).

On your Linux host computer, open a terminal window, locate the source
code of this sample application (i.e., `examples/zephyr/lightdb/set`) and type:

```console
$ west build -b qemu_x86 examples/zephyr/lightdb/set
$ west build -t run
```

See [Networking with
QEMU](https://docs.zephyrproject.org/3.3.0/connectivity/networking/qemu_setup.html)
on how to setup networking on host and configure NAT/masquerading to
access Internet.

#### nRF52840 DK + ESP32-WROOM-32

This subsection documents using nRF52840 DK running Zephyr with
offloaded ESP-AT WiFi driver and ESP32-WROOM-32 module based board (such
as ESP32 DevkitC rev. 4) running WiFi stack. See [AT Binary
Lists](https://docs.espressif.com/projects/esp-at/en/latest/AT_Binary_Lists/index.html)
for links to ESP-AT binaries and details on how to flash ESP-AT image on
ESP chip. Flash ESP chip with following command:

```console
esptool.py write_flash --verify 0x0 PATH_TO_ESP_AT/factory/factory_WROOM-32.bin
```

Connect nRF52840 DK and ESP32-DevKitC V4 (or other ESP32-WROOM-32 based
board) using wires:

|             |                |
| ----------- | -------------- |
| nRF52840 DK | ESP32-WROOM-32 |
| P1.01 (RX)  | IO17 (TX)      |
| P1.02 (TX)  | IO16 (RX)      |
| P1.03 (CTS) | IO14 (RTS)     |
| P1.04 (RTS) | IO15 (CTS)     |
| P1.05       | EN             |
| GND         | GND            |

Configure the following Kconfig options based on your WiFi AP
credentials:

* GOLIOTH_SAMPLE_WIFI_SSID - WiFi SSID
* GOLIOTH_SAMPLE_WIFI_PSK - WiFi PSK

by adding these lines to configuration file (e.g. `prj.conf` or
`board/nrf52840dk_nrf52840.conf`):

```cfg
CONFIG_GOLIOTH_SAMPLE_WIFI_SSID="my-wifi"
CONFIG_GOLIOTH_SAMPLE_WIFI_PSK="my-psk"
```

On your host computer open a terminal window, locate the source code of
this sample application (i.e., `examples/zephyr/lightdb/set`) and type:

```console
$ west build -b nrf52840dk_nrf52840 examples/zephyr/lightdb/set
$ west flash
```

#### nRF9160 DK

On your host computer open a terminal window, locate the source code of
this sample application (i.e., `samples/ligthdb/set`) and type:

```console
$ west build -b nrf9160dk_nrf9160_ns examples/zephyr/lightdb/set
$ west flash
```

### Sample output

This is the output from the serial console:

```console
[00:00:00.000,000] <inf> golioth_system: Initializing
[00:00:00.000,000] <inf> net_config: Initializing network
[00:00:00.000,000] <inf> net_config: IPv4 address: 192.0.2.1
[00:00:00.000,000] <dbg> golioth_lightdb: main: Start LightDB set sample
[00:00:00.010,000] <inf> golioth_system: Starting connect
[00:00:00.030,000] <dbg> golioth_lightdb: main: Setting counter to 0
[00:00:00.030,000] <dbg> golioth_lightdb: main: Before request (async)
[00:00:00.030,000] <dbg> golioth_lightdb: main: After request (async)
[00:00:00.030,000] <inf> golioth_system: Client connected!
[00:00:00.030,000] <dbg> golioth_lightdb: counter_set_handler: Counter successfully set
[00:00:05.040,000] <dbg> golioth_lightdb: main: Setting counter to 1
[00:00:05.040,000] <dbg> golioth_lightdb: main: Before request (sync)
[00:00:05.040,000] <dbg> golioth_lightdb: counter_set_sync: Counter successfully set
[00:00:05.040,000] <dbg> golioth_lightdb: main: After request (sync)
[00:00:10.050,000] <dbg> golioth_lightdb: main: Setting counter to 2
[00:00:10.050,000] <dbg> golioth_lightdb: main: Before request (async)
[00:00:10.050,000] <dbg> golioth_lightdb: main: After request (async)
[00:00:10.050,000] <dbg> golioth_lightdb: counter_set_handler: Counter successfully set
[00:00:15.060,000] <dbg> golioth_lightdb: main: Setting counter to 3
[00:00:15.060,000] <dbg> golioth_lightdb: main: Before request (sync)
[00:00:15.060,000] <dbg> golioth_lightdb: counter_set_sync: Counter successfully set
[00:00:15.060,000] <dbg> golioth_lightdb: main: After request (sync)
```

### Monitor counter value

Device increments counter every 5s and updates `/counter` resource in
LightDB with its value. Current value can be fetched using following
command:

```console
goliothctl lightdb get <device-name> /counter
```