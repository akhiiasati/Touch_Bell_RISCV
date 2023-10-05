# Touch_Sensor_Based_Bell_RISCV

## Introduction

A capacitive touch sensor module based on the dedicated TTP223 touch sensor IC. The module provides a single integrated touch sensing area of 11 x 10.5mm with a sensor range of ~5mm. An on-board LED will give a visual indication of when the sensor is triggered. When triggered the module’s output will switch from its idle low state to high (default operation). Solder jumpers allow for reconfiguring its mode of operation to be either active low or toggle output.

## Internal Structure of TTP223

TTP223 is 1 Key Touchpad detector IC, and it is suitable to detect capacitive element variations. It consumes very low power and the operating voltage is only between 2.0V~5.5V. The response time max about 60mS at fast mode, 220mS at low power mode @VDD=3V. Sensitivity can adjust by the capacitance(0~50pF) outside.

![download](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/49fbe971-92f4-46a8-874e-692b2a5ea024)

## Working Mechanism of TTP223 Capacitive Touch Sensor

Capacitive screens do not use the pressure of your finger to create a change in the flow of electricity. Instead, they work with anything that holds an electrical charge – including human skin.

When a finger hits the screen a tiny electrical charge is transferred to the finger to complete the circuit, creating a voltage drop on that point of the screen. The software processes the location of this voltage drop and orders the ensuing action.
