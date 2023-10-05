# Touch_Sensor_Based_Bell_RISCV

## Introduction

The Touch Bell is a simple and interactive project that uses the TTP223 capacitive touch sensor and RISCV based processor to create a touch-activated bell. When a user touches the sensor, it triggers a bell sound, making it a fun and engaging way to call for attention or alert someone.

## TTP223 Capacitive Touch Sensor Switch

The TTP223 Capacitive Touch Sensor Switch is a touch-sensitive sensor module that uses capacitive touch technology to detect when it is touched or approached by a conductive object, such as a human finger. This type of sensor is widely used in various electronic applications for creating touch-sensitive interfaces or switches.

Here are some key features and characteristics of the TTP223 Capacitive Touch Sensor Switch:

- Touch Sensing: The TTP223 sensor can detect touch without any physical pressure. When a conductive object (like a finger) comes close to or makes contact with the sensor pad, it triggers a response.

- Digital Output: It provides a digital output, typically in the form of a HIGH (logic 1) or LOW (logic 0) signal, indicating whether a touch has been detected or not.

- Simple Interface: It's easy to interface with microcontrollers like Arduino, Raspberry Pi, or other digital circuits. You usually connect the sensor's output pin to a digital input pin on your microcontroller.

- Low Power: The TTP223 sensor is designed to be energy-efficient, making it suitable for battery-powered applications.

- Sensitivity Adjustment: Some TTP223 modules may allow you to adjust the sensitivity of the touch detection, allowing you to fine-tune the response based on your requirements.

- Applications: It can be used in a wide range of applications, including touch-sensitive buttons, switches, interactive displays, proximity sensors, and more.

- Variants: There are different variants of the TTP223 sensor with varying features and pin configurations. The TTP223-BA6 is a common variant that provides a single touch-sensitive pad.

- Operating Voltage: TTP223 sensors typically operate at 3.3V or 5V, depending on the model and manufacturer.

- Reliability: Capacitive touch sensors like the TTP223 are known for their reliability and durability, as they have no moving parts and are not subject to wear and tear.

- Cost-Effective: These sensors are generally cost-effective and readily available, making them a popular choice for hobbyists and engineers.

The TTP223 Capacitive Touch Sensor Switch is frequently used in DIY electronics projects, including touch-sensitive lamps, touch-controlled appliances, interactive art installations, and more. It provides an intuitive and convenient way to interact with devices and interfaces without the need for physical buttons or switches.

![download](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/49fbe971-92f4-46a8-874e-692b2a5ea024)

## Working Mechanism of TTP223 Capacitive Touch Sensor

Capacitive screens do not use the pressure of your finger to create a change in the flow of electricity. Instead, they work with anything that holds an electrical charge – including human skin.

When a finger hits the screen a tiny electrical charge is transferred to the finger to complete the circuit, creating a voltage drop on that point of the screen. The software processes the location of this voltage drop and orders the ensuing action.

![Untitled-9](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/874d48ac-b205-44dc-ab7d-dba6a7e8738a)


## C Code

```C
//#include <stdio.h>

//#define ctsPin 7 // Pin for capacitive touch sensor
//#define ledPin 13 // Pin for the LED

int main() {

    //pinMode(ledPin, OUTPUT);
    //pinMode(ctsPin, INPUT);
    int ctsPin=0;
    int ledPin=0;
    
    asm(
	"or x30, x30, %0\n\t" 
	:"=r"(ctsPin));
    
    asm(
	"andi %0, x30, 1\n\t"
	:"=r"(ledPin));
    
    while (1) {
        //int ctsValue = digitalRead(ctsPin);
        if (ctsPin) {
            ledPin = 1;
            // Use printf to indicate "TOUCHED" without Serial
            //printf("TOUCHED\n");
        } else {
            ledPin=0;
            // Use printf to indicate "not touched" without Serial
            printf("not touched\n");
        }
        delay(500); // Delay for 500 milliseconds (0.5 seconds)
    }

    return 0;
}
```
