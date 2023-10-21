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

## Testing

We first compile the C code using gcc and check the expected outcomes for testcases.

```bash
gcc locker.c
./a.out
```

![Screenshot from 2023-10-10 22-18-18](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/c55f0b41-3cf5-4ca5-adb0-b84d7cc2d059)
![Screenshot from 2023-10-10 22-17-44](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/7f3667fe-690e-4482-aaf0-ed3a1d3ba924)

## C Code

```C
void initialize();
void delaytime(int);
int main()
{

int TouchSensor;
int BuzzerPin;
int BuzzerPin_reg;

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

## Assembly Code
```assembly

out:     file format elf32-littleriscv


Disassembly of section .text:

00010054 <main>:
   10054:	fe010113          	addi	sp,sp,-32
   10058:	00112e23          	sw	ra,28(sp)
   1005c:	00812c23          	sw	s0,24(sp)
   10060:	02010413          	addi	s0,sp,32
   10064:	fe042623          	sw	zero,-20(s0)
   10068:	fec42783          	lw	a5,-20(s0)
   1006c:	00179793          	slli	a5,a5,0x1
   10070:	fef42423          	sw	a5,-24(s0)
   10074:	fe842783          	lw	a5,-24(s0)
   10078:	00ff6f33          	or	t5,t5,a5
   1007c:	001f7793          	andi	a5,t5,1
   10080:	fef42223          	sw	a5,-28(s0)
   10084:	fe442783          	lw	a5,-28(s0)
   10088:	02078663          	beqz	a5,100b4 <main+0x60>
   1008c:	00100793          	li	a5,1
   10090:	fef42623          	sw	a5,-20(s0)
   10094:	fec42783          	lw	a5,-20(s0)
   10098:	00179793          	slli	a5,a5,0x1
   1009c:	fef42423          	sw	a5,-24(s0)
   100a0:	fe842783          	lw	a5,-24(s0)
   100a4:	00ff6f33          	or	t5,t5,a5
   100a8:	3e800513          	li	a0,1000
   100ac:	024000ef          	jal	ra,100d0 <delaytime>
   100b0:	fcdff06f          	j	1007c <main+0x28>
   100b4:	fe042623          	sw	zero,-20(s0)
   100b8:	fec42783          	lw	a5,-20(s0)
   100bc:	00179793          	slli	a5,a5,0x1
   100c0:	fef42423          	sw	a5,-24(s0)
   100c4:	fe842783          	lw	a5,-24(s0)
   100c8:	00ff6f33          	or	t5,t5,a5
   100cc:	fb1ff06f          	j	1007c <main+0x28>

000100d0 <delaytime>:
   100d0:	fd010113          	addi	sp,sp,-48
   100d4:	02812623          	sw	s0,44(sp)
   100d8:	03010413          	addi	s0,sp,48
   100dc:	fca42e23          	sw	a0,-36(s0)
   100e0:	fe042623          	sw	zero,-20(s0)
   100e4:	0340006f          	j	10118 <delaytime+0x48>
   100e8:	fe042423          	sw	zero,-24(s0)
   100ec:	0100006f          	j	100fc <delaytime+0x2c>
   100f0:	fe842783          	lw	a5,-24(s0)
   100f4:	00178793          	addi	a5,a5,1
   100f8:	fef42423          	sw	a5,-24(s0)
   100fc:	fe842703          	lw	a4,-24(s0)
   10100:	000f47b7          	lui	a5,0xf4
   10104:	23f78793          	addi	a5,a5,575 # f423f <__global_pointer$+0xe290b>
   10108:	fee7d4e3          	bge	a5,a4,100f0 <delaytime+0x20>
   1010c:	fec42783          	lw	a5,-20(s0)
   10110:	00178793          	addi	a5,a5,1
   10114:	fef42623          	sw	a5,-20(s0)
   10118:	fec42703          	lw	a4,-20(s0)
   1011c:	fdc42783          	lw	a5,-36(s0)
   10120:	fcf744e3          	blt	a4,a5,100e8 <delaytime+0x18>
   10124:	00000013          	nop
   10128:	02c12403          	lw	s0,44(sp)
   1012c:	03010113          	addi	sp,sp,48
   10130:	00008067          	ret
```

## RISCV Instruction in Assembly Code

```
Number of different instructions: 15
List of unique instructions:
li
bge
blt
slli
sw
addi
lw
or
nop
jal
ret
beqz
j
lui
andi
```
