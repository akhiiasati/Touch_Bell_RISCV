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

## Assembly Code
```assembly

akhil1.o:     file format elf32-littleriscv

Disassembly of section .text:

00010094 <main>:
   10094:	ff010113          	add	sp,sp,-16
   10098:	00112623          	sw	ra,12(sp)
   1009c:	00812423          	sw	s0,8(sp)
   100a0:	01010413          	add	s0,sp,16
   100a4:	000117b7          	lui	a5,0x11
   100a8:	1807ae23          	sw	zero,412(a5) # 1119c <BuzzerPin>
   100ac:	000117b7          	lui	a5,0x11
   100b0:	19c7a783          	lw	a5,412(a5) # 1119c <BuzzerPin>
   100b4:	00179713          	sll	a4,a5,0x1
   100b8:	80e1a423          	sw	a4,-2040(gp) # 111a0 <BuzzerPin_reg>
   100bc:	8081a783          	lw	a5,-2040(gp) # 111a0 <BuzzerPin_reg>
   100c0:	00ff6f33          	or	t5,t5,a5
   100c4:	001f7713          	and	a4,t5,1
   100c8:	000117b7          	lui	a5,0x11
   100cc:	18e7ac23          	sw	a4,408(a5) # 11198 <__DATA_BEGIN__>
   100d0:	000117b7          	lui	a5,0x11
   100d4:	1987a783          	lw	a5,408(a5) # 11198 <__DATA_BEGIN__>
   100d8:	02078a63          	beqz	a5,1010c <main+0x78>
   100dc:	000117b7          	lui	a5,0x11
   100e0:	00100713          	li	a4,1
   100e4:	18e7ae23          	sw	a4,412(a5) # 1119c <BuzzerPin>
   100e8:	000117b7          	lui	a5,0x11
   100ec:	19c7a783          	lw	a5,412(a5) # 1119c <BuzzerPin>
   100f0:	00179713          	sll	a4,a5,0x1
   100f4:	80e1a423          	sw	a4,-2040(gp) # 111a0 <BuzzerPin_reg>
   100f8:	8081a783          	lw	a5,-2040(gp) # 111a0 <BuzzerPin_reg>
   100fc:	00ff6f33          	or	t5,t5,a5
   10100:	3e800513          	li	a0,1000
   10104:	02c000ef          	jal	10130 <delaytime>
   10108:	fbdff06f          	j	100c4 <main+0x30>
   1010c:	000117b7          	lui	a5,0x11
   10110:	1807ae23          	sw	zero,412(a5) # 1119c <BuzzerPin>
   10114:	000117b7          	lui	a5,0x11
   10118:	19c7a783          	lw	a5,412(a5) # 1119c <BuzzerPin>
   1011c:	00179713          	sll	a4,a5,0x1
   10120:	80e1a423          	sw	a4,-2040(gp) # 111a0 <BuzzerPin_reg>
   10124:	8081a783          	lw	a5,-2040(gp) # 111a0 <BuzzerPin_reg>
   10128:	00ff6f33          	or	t5,t5,a5
   1012c:	f99ff06f          	j	100c4 <main+0x30>

00010130 <delaytime>:
   10130:	fd010113          	add	sp,sp,-48
   10134:	02812623          	sw	s0,44(sp)
   10138:	03010413          	add	s0,sp,48
   1013c:	fca42e23          	sw	a0,-36(s0)
   10140:	fe042623          	sw	zero,-20(s0)
   10144:	0340006f          	j	10178 <delaytime+0x48>
   10148:	fe042423          	sw	zero,-24(s0)
   1014c:	0100006f          	j	1015c <delaytime+0x2c>
   10150:	fe842783          	lw	a5,-24(s0)
   10154:	00178793          	add	a5,a5,1
   10158:	fef42423          	sw	a5,-24(s0)
   1015c:	fe842703          	lw	a4,-24(s0)
   10160:	000f47b7          	lui	a5,0xf4
   10164:	23f78793          	add	a5,a5,575 # f423f <__global_pointer$+0xe28a7>
   10168:	fee7d4e3          	bge	a5,a4,10150 <delaytime+0x20>
   1016c:	fec42783          	lw	a5,-20(s0)
   10170:	00178793          	add	a5,a5,1
   10174:	fef42623          	sw	a5,-20(s0)
   10178:	fec42703          	lw	a4,-20(s0)
   1017c:	fdc42783          	lw	a5,-36(s0)
   10180:	fcf744e3          	blt	a4,a5,10148 <delaytime+0x18>
   10184:	00000013          	nop
   10188:	00000013          	nop
   1018c:	02c12403          	lw	s0,44(sp)
   10190:	03010113          	add	sp,sp,48
   10194:	00008067          	ret
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
