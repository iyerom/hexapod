# Hexapod Robot

A 3D-printed walking robot originally designed as a hexapod. Built around servo-driven legs, a dedicated PWM driver stage, and a migration from Arduino to ESP32 for onboard wireless control. Gait and leg kinematics were also prototyped in a Unity simulation before being carried over to the physical build.

![Fully built robot](https://github.com/user-attachments/assets/8fed00e0-3306-42f1-b642-271da800b991)  
*previous build, fully assembled*

## Status

**In progress**: reviving a project after ~1 year on pause. Currently mid-migration from Arduino Uno to ESP32 DevKitC, in the process of reprinting all leg and body models.

## Overview

- **Design:** 6-legged hexapod, 3 servos per leg (18x MG996S)
- **Controller migration:** Arduino Uno -> ESP32 DevKitC
- **Why ESP32:** built-in WiFi/BLE removes the need for a separate wireless module, much MUCH more computation power

## Hardware

| Component | Part | Notes |
|---|---|---|
| Microcontroller | ESP32 DevKitC | replacing Arduino Uno |
| Servos | 18x MG996S | 3 per leg, coxa, tibia, femur joints |
| Servo driver | 2x PCA9685 | I2C PWM expansion |
| Display | GMT020-02-7P 2.0" TFT SPI | might change to oled |
| Power input | 7.4V LiPo | main pack |
| Voltage regulation | LM2596 / XL4015-class buck converter | Steps 7.4V down to ~6V for servo rail |
| Wireless | Onboard ESP32 WiFi/BLE | replaces removed HC-05 Bluetooth module |
  
  
Key design decisions:
- **Rail isolation:** the ESP32 is powered from its own dedicated small buck module tapped off the main buck's input, rather than sharing the servo output rail, avoiding brownouts from servo current spikes.
- **PCA9685 power split:** logic (`VCC`) is fed from the ESP32's 3.3V line for correct I2C level matching; the servo rail (`V+`) is separate and can run up to 6V.
- **Why:** Isolating its supply was necessary for reliable operation under servo load to avoid triggering the ESP32's brownout detection.

## Migration Notes (Arduino -> ESP32)

- Removing the HC-05 Bluetooth module entirely as the ESP32's onboard wireless makes it redundant.
- Potentially changing the TFT display to an OLED for real-time animations rather than static face.
- Rebuilding the power distribution to give the microcontroller an isolated supply

## Simulation

Before committing to the physical build, gait patterns and leg kinematics were prototyped in a Unity simulation to validate movement logic ahead of hardware.

![Unity simulation](https://github.com/user-attachments/assets/dc9ee95e-e21c-4e50-a68a-48b4a0adb41d)  
*Leg kinematics/gait simulation in Unity*




## Media

**Individual leg mechanism**

https://github.com/user-attachments/assets/92b3b6fd-e322-4a81-b554-cf44252994d0

*single leg tested in isolation*

**Prior prototype - full gait**

https://github.com/user-attachments/assets/6f3593b4-761a-4cba-86d1-77666a1db1fa  
*Earlier prototype with gait cycle*

## Roadmap

- [ ] Reprint all leg and body models
- [ ] Assemble all legs and wire all components  
- [ ] Complete ESP32 migration and validate gait control over the new controller  
- [ ] Wireless control interface (leveraging onboard WiFi/BLE)  

## Background

This project is a return to an earlier build that was shelved for about a year. The current phase is focused on modernizing the electronics (controller + power system).

---
