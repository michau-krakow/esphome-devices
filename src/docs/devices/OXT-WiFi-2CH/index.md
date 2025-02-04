---
title: OXT WiFi 2 Ch Relay Module
date-published: 2024-11-01
type: relay
standard: global
board: bk72xx
made-for-esphome: False
difficulty: 4
---

## About

OXT is Polish brand offering home-automation dimmers, switches, etc. based on Tuya framework.
Their 2-channel relay switch is also Tuya based device using CB2S module.

## Product Images

<!-- ![main view](/device.jpg "DEVICE")
![pcb](/pcb.jpg "PCB") -->

## GPIO Pinout

| Pin       | Function   |
|-----------|------------|
| P6        | Status LED |
| P7        | L2 Relay   |
| P8        | K1 Button  |
| ADC (P23) | L1 Relay   |
| P24       | S2 Input   |
| P26       | S1 Input   |

## Basic Configuration

```yaml
substitutions:
  device_name: "oxt-2ch"
  
esphome:
  name: oxt-2ch
  comment: OXT 2-ch Relay

bk72xx:
  board: cb2s

status_led:
  pin:
    number: P6
    inverted: true

switch:
  - platform: gpio
    id: relay1
    name: Relay1
    pin: P23
  - platform: gpio
    id: relay2
    name: Relay2
    pin: P7

binary_sensor:
  - platform: gpio
    id: input_s1
    name: Input S1
    pin:
      number: P26
      mode: INPUT_PULLUP
      inverted: true
  - platform: gpio
    id: input_s2
    name: Input S2
    pin:
      number: P24
      mode: INPUT_PULLUP
      inverted: true
  - platform: gpio
    id: button1
    name: Button K1
    pin:
      number: P8
      mode: INPUT_PULLUP
      inverted: true
    on_press:
      - switch.toggle: relay1
      - switch.toggle: relay2
```

## Programming

<!-- Some soldering is required to  -->
<!-- ![soldering](/soldering.jpg "soldering") -->

Short CEN to GND to enter bootloader mode, use `ltchiptool` to reprogram it for the first time:

```bash
I: Connect UART1 of the BK7231 to the USB-TTL adapter:
I: 
I:     --------+        +--------------------
I:          PC |        | BK7231             
I:     --------+        +--------------------
I:          RX | ------ | TX1 (GPIO11 / P11) 
I:          TX | ------ | RX1 (GPIO10 / P10) 
I:             |        |                    
I:         GND | ------ | GND                
I:     --------+        +--------------------
I:  
I: Using a good, stable 3.3V power supply is crucial. Most flashing issues
I: are caused by either voltage drops during intensive flash operations,
I: or bad/loose wires.
I:  
I: To enter download mode, the chip has to be rebooted while the flashing program
I: is trying to establish communication.
I: In order to do that, you need to bridge CEN pin to GND with a wire.
I: |-- Success! Chip info: BK7231N
I: Reading chip info...
I: Chip: BK7231N
D: Reading 4k page at 0x1D0000 (0.00%)
D: Flash size - detecting...
D:  - Checking wraparound at 0x211000
D: Flash size detected - 0x200000
```
