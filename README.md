# esphome-fchao


ESPHome example configuration to monitor and control a FCHAO 030KHP hybrid inverter via rs485

## Supported devices

* FCHAO 030KHP5.5KW-48V - FCHAO available on aliexpress

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/IMG_6739.jpeg" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/IMG_6739.jpeg" height="250">
</a>

## Requirements

* [ESPHome 2026.4.0 or higher](https://github.com/esphome/esphome/releases).
* Plugs provided by inverter distributor (attached in package)
* LilyGo-RS485/CAN or self assembled Generic ESP32 board with RS485toTTL converter 

## Schematics

Inverter provides rs485 pins A and B on additional connector and this can work in parallell with attached data logger RWB1-51

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/IMG_6740.jpeg" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/IMG_6740.jpeg" height="250">
</a>

<B>Red wire is A and Black is B for modbus/rs485 connection</B>

There is also possibility to use rj45 socket for RWB1 data logger to connect esphome device following pinout below but this will require disconnecting data logger unless using rj45 splitter:

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/Solar-Plug-RWB1-rj45-pinout.png" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/Solar-Plug-RWB1-rj45-pinout.png" height="250">
</a>

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/Solar-Plug-RWB1-rj45-plug.png" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/Solar-Plug-RWB1-rj45-plug.png" height="250">
</a>

```
LilyGo-RS485/CAN version

                  rs485                   
┌──────────┐                 ┌──────────┐      
│          │                 │          │
│          │<─────  A  ─────>│  LilyGo  │
│  FCHAO   │<─────  B  ─────>│  rs485   │
│  030KHP  │<───── GND ─────>│  / CAN   │<── VCC ──┐
│          │<─┐         │    │          │<── GND ┐ │ 
└──────────┘  │         │    └──────────┘        │ │       
              |         └────────────────────────┘ │ 
              └─ 5V ───────────────────────────────┘  
              
```               

```
self assembled Generic ESP32 board with RS485toTTL converter 
  
                  rs485                        UART-TTL           
┌──────────┐                 ┌──────────┐                   ┌──────────┐
│          │                 │          │                   │          │
│          │<─────  A  ─────>│  rs485   │<───  RX - TX  ───>│          │
│  FCHAO   │<─────  B  ─────>│  to      │<───  TX - RX  ───>│  ESP32   │
│  030KHP  │<───── GND ─────>│  TTL     │<─────  GND  ─────>│          │<────── VCC ──┐
│          │<─┐         │    │          │<─── 3.3V VCC ────>│          │<────── GND ┐ │ 
└──────────┘  │         │    └──────────┘                   └──────────┘            │ │       
              |         └───────────────────────────────────────────────────────────┘ │ 
              └─ 5V ──────────────────────────────────────────────────────────────────┘  

```

The inverter provides VCC (+8V) and GND on pin 3 and 4 of "hidden" rs485 connector but struggles with power to start lilygo. (See picture below for pinout). 

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/IMG_6740.jpeg" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/IMG_6740.jpeg" height="250">
</a>

<B>Yellow wire is GND and Blue is VCC (+8V)</B>

For simplicity you can buy 4pin JST XH connector

Also you can use a cheap DC-DC converter to power the ESP with 8V input and 3.3V/5V output.

## Installation in home assistant

Use the `esphome-fchao-030khp.yaml` for testing purposes and finetune as of your needs

There is several commented  out sensors due to significant amont of them. Feel free to uncomment them if you need them.

```bash
# Install esphome addon in home assistant


# Create new device in esphome plugin using "Empty configuration" option and copy and paste content of yaml file

# Create a secret.yaml containing some setup specific secrets

wifi_ssid: YOUR_WIFI_SSID
wifi_password: YOUR_WIFI_PASSWORD

api_key: YOUR_API_KEY
ota_password: YOUR_OTA_PASSWORD

# Validate the configuration, create a binary, upload it, and start logs

```

## Known issues

1. This is work in progress but majority sensors works out of the box keep that in mind when using automations

2. Not tested on esp8266 but most likely with proper configuration and build it will work

3. Tried to use VCC and GND to power LilyGo but was unsuccessful as it looks that LilyGo was draining more power than available.


## Work in progress
1. Work got completed for major registers. Currently reverse engineering fault and warning codes in modbus registers

2. Testing, testing, testing. Looking for volenteers ;)  - a lot already tested

