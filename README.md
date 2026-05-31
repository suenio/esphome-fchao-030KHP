# esphome-fchao


ESPHome example configuration to monitor and control a FCHAO 030KHP hybrid inverter via rs485

## Supported devices

* FCHAO 030KHP5.5KW-48V - FCHAO available on aliexpress


## Requirements

* [ESPHome 2026.4.0 or higher](https://github.com/esphome/esphome/releases).
* Plugs provided by inverter distributor (attached in package)
* LilyGo-RS485/CAN or self assembled Generic ESP32 board with RS485toTTL converter 

## Schematics

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/jsdsolar-img2.png" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/jsdsolar-img2.png" height="250">
</a>

Inverter provides rs485 pins A and B on additional connector and this can work in parallell with attahced data logger RWB1-51

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/jsdsolar-img1.png" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/jsdsolar-img1.png" height="250">
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

The inverter provides +8V and GND on pin 3 and 4 of "hidden" rs485 connector but struggles with power to start lilygo. (See picture below for pinout). 

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/fchao-img4.jpeg" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/fchao-img4.jpeg" height="400">
</a>

For simplicity you can buy 4pin JST XH connector

Also you can use a cheap DC-DC converter to power the ESP with 8V input and 3.3V/5V output.

Finally your results after installing esphome code on the lilygo device will be:

<a href="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/fchao-img3.png" target="_blank">
  <img src="https://github.com/suenio/esphome-fchao-030khp/blob/main/images/fchao-img3.png" height="1200">
</a>

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


## Work in progress
1. Work got completed for major registers. Currently reverse engineering fault and warning codes in modbus registers

2. Testing, testing, testing. Looking for volenteers ;)  - a lot already tested

