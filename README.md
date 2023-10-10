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


## Block Diagram

![Block](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/0ba9bc5d-8d01-459b-b538-e5d753ef6dc4)



## C Code

```C
int TouchSensor;
int BuzzerPin;
int BuzzerPin_reg;

void initialize();
void delaytime(int);
int main()
{

BuzzerPin =0;
BuzzerPin_reg = BuzzerPin*2;


asm volatile(
	"or x30, x30, %0\n\t" 
	:
	:"r"(BuzzerPin_reg)
	:"x30"
	);



while (1) {


//  asm code to read sensor value
asm volatile(
	"andi %0, x30, 1\n\t"
	:"=r"(TouchSensor)
	:
	:
	);




if (TouchSensor)
	{
	BuzzerPin=1;
	BuzzerPin_reg = BuzzerPin*2;
	
	//asm code to set output reg
	asm volatile(
	"or x30, x30, %0\n\t" 
	:
	:"r"(BuzzerPin_reg)
	:"x30"
	);


	delaytime(1000);
	}
else
	{
	BuzzerPin=0;
	BuzzerPin_reg = BuzzerPin*2;
	
	//asm code to set output reg	
	asm volatile(
	"or x30, x30, %0\n\t" 
	:
	:"r"(BuzzerPin_reg)
	:"x30"
	);

	}

}
delaytime(500);
return 0;
}

void delaytime(int seconds) {
    int i, j;
    for (i = 0; i < seconds; i++) {
        for (j = 0; j < 1000000; j++) {
            //loop for delay
        }
    }
}
```
![Screenshot from 2023-10-10 22-18-18](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/c55f0b41-3cf5-4ca5-adb0-b84d7cc2d059)
![Screenshot from 2023-10-10 22-17-44](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/7f3667fe-690e-4482-aaf0-ed3a1d3ba924)

## Assembly Code
```assembly

out:     file format elf32-littleriscv


Disassembly of section .text:

00000000 <main>:
   0: ff010113                add   sp,sp,-16
   4: 00112623                sw    ra,12(sp)
   8: 00812423                sw    s0,8(sp)
   c: 01010413                add   s0,sp,16
  10: 000007b7                lui   a5,0x0
  14: 0007a023                sw    zero,0(a5) # 0 <main>
  18: 000007b7                lui   a5,0x0
  1c: 0007a783                lw    a5,0(a5) # 0 <main>
  20: 00179713                sll   a4,a5,0x1
  24: 000007b7                lui   a5,0x0
  28: 00e7a023                sw    a4,0(a5) # 0 <main>
  2c: 000007b7                lui   a5,0x0
  30: 0007a783                lw    a5,0(a5) # 0 <main>
  34: 00ff6f33                or    t5,t5,a5

00000038 <.L4>:
  38: 001f7713                and   a4,t5,1
  3c: 000007b7                lui   a5,0x0
  40: 00e7a023                sw    a4,0(a5) # 0 <main>
  44: 000007b7                lui   a5,0x0
  48: 0007a783                lw    a5,0(a5) # 0 <main>
  4c: 04078063                beqz  a5,8c <.L2>
  50: 000007b7                lui   a5,0x0
  54: 00100713                li    a4,1
  58: 00e7a023                sw    a4,0(a5) # 0 <main>
  5c: 000007b7                lui   a5,0x0
  60: 0007a783                lw    a5,0(a5) # 0 <main>
  64: 00179713                sll   a4,a5,0x1
  68: 000007b7                lui   a5,0x0
  6c: 00e7a023                sw    a4,0(a5) # 0 <main>
  70: 000007b7                lui   a5,0x0
  74: 0007a783                lw    a5,0(a5) # 0 <main>
  78: 00ff6f33                or    t5,t5,a5
  7c: 3e800513                li    a0,1000
  80: 00000097                auipc ra,0x0
  84: 000080e7                jalr  ra # 80 <.L4+0x48>
  88: fb1ff06f                j     38 <.L4>

0000008c <.L2>:
  8c: 000007b7                lui   a5,0x0
  90: 0007a023                sw    zero,0(a5) # 0 <main>
  94: 000007b7                lui   a5,0x0
  98: 0007a783                lw    a5,0(a5) # 0 <main>
  9c: 00179713                sll   a4,a5,0x1
  a0: 000007b7                lui   a5,0x0
  a4: 00e7a023                sw    a4,0(a5) # 0 <main>
  a8: 000007b7                lui   a5,0x0
  ac: 0007a783                lw    a5,0(a5) # 0 <main>
  b0: 00ff6f33                or    t5,t5,a5
  b4: f85ff06f                j     38 <.L4>

000000b8 <delaytime>:
  b8: fd010113                add   sp,sp,-48
  bc: 02812623                sw    s0,44(sp)
  c0: 03010413                add   s0,sp,48
  c4: fca42e23                sw    a0,-36(s0)
  c8: fe042623                sw    zero,-20(s0)
  cc: 0340006f                j     100 <.L6>

000000d0 <.L9>:
  d0: fe042423                sw    zero,-24(s0)
  d4: 0100006f                j     e4 <.L7>

000000d8 <.L8>:
  d8: fe842783                lw    a5,-24(s0)
  dc: 00178793                add   a5,a5,1
  e0: fef42423                sw    a5,-24(s0)

000000e4 <.L7>:
  e4: fe842703                lw    a4,-24(s0)
  e8: 000f47b7                lui   a5,0xf4
  ec: 23f78793                add   a5,a5,575 # f423f <.L6+0xf413f>
  f0: fee7d4e3                bge   a5,a4,d8 <.L8>
  f4: fec42783                lw    a5,-20(s0)
  f8: 00178793                add   a5,a5,1
  fc: fef42623                sw    a5,-20(s0)

00000100 <.L6>:
 100: fec42703                lw    a4,-20(s0)
 104: fdc42783                lw    a5,-36(s0)
 108: fcf744e3                blt   a4,a5,d0 <.L9>
 10c: 00000013                nop
 110: 00000013                nop
 114: 02c12403                lw    s0,44(sp)
 118: 03010113                add   sp,sp,48
 11c: 00008067                ret
```

## RISCV Instruction in Assembly Code

```
Number of different instructions: 16
List of unique instructions:
lw
beqz
lui
and
li
add
sll
nop
blt
j
sw
or
auipc
jalr
ret
bge
```
