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
//#include<stdio.h>

int main()
{
int i,j;
int touchsensor_value,Result1,mask;
int touchsensor,buzzer;



//for (int j=0; j<15;j++) 

while (1)
{

/*if(j%3==1)
			touchsensor_value = 1;
	else
			touchsensor_value =0;
*/			


			
//  asm code to read sensor value

	asm volatile(
		"andi %0, x30, 0x01\n\t"
		: "=r" (touchsensor_value)
		:
		: 
		);



//if condition logic
if (touchsensor_value)
	{
	mask=0xFFFFFFFD;
	
	// printf(" \n");
	
	buzzer=1;
	
	asm volatile(    
            "ori x30, x30,2"               
            :
            :
            :"x30"
            );
            
            asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(Result1)
	    	:
	    	:"x30"
	    	);
    	//printf("Result1 = %d\n",Result1);
    	
	

	}
else
	{
	
	mask=0xFFFFFFFD;
	
	buzzer=0;
	
	
	asm volatile( 
            "and x30,x30, %0\n\t"     
            "ori x30, x30,0"            
            :
            :"r"(mask)
            :"x30"
        );
        asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(Result1)
	    	:
	    	:"x30"
	    	);
	 //printf("Result1 = %d\n",Result1);

	}
	//printf("buzzer=%d \n", touchsensor_value); 
}

return 0;
}
```

## Assembly Code
```assembly

out:     file format elf32-littleriscv


Disassembly of section .text:

00010054 <main>:
   10054:	fe010113          	addi	sp,sp,-32
   10058:	00812e23          	sw	s0,28(sp)
   1005c:	02010413          	addi	s0,sp,32
   10060:	001f7793          	andi	a5,t5,1
   10064:	fef42623          	sw	a5,-20(s0)
   10068:	fec42783          	lw	a5,-20(s0)
   1006c:	02078263          	beqz	a5,10090 <main+0x3c>
   10070:	ffd00793          	li	a5,-3
   10074:	fef42423          	sw	a5,-24(s0)
   10078:	00100793          	li	a5,1
   1007c:	fef42223          	sw	a5,-28(s0)
   10080:	002f6f13          	ori	t5,t5,2
   10084:	000f0793          	mv	a5,t5
   10088:	fef42023          	sw	a5,-32(s0)
   1008c:	fd5ff06f          	j	10060 <main+0xc>
   10090:	ffd00793          	li	a5,-3
   10094:	fef42423          	sw	a5,-24(s0)
   10098:	fe042223          	sw	zero,-28(s0)
   1009c:	fe842783          	lw	a5,-24(s0)
   100a0:	00ff7f33          	and	t5,t5,a5
   100a4:	000f6f13          	ori	t5,t5,0
   100a8:	000f0793          	mv	a5,t5
   100ac:	fef42023          	sw	a5,-32(s0)
   100b0:	fb1ff06f          	j	10060 <main+0xc>
```

## RISCV Instruction in Assembly Code

```
Number of different instructions: 10
List of unique instructions:
j
andi
addi
sw
beqz
ori
and
lw
mv
li
```
## Spike Code:

```C
#include<stdio.h>

int main()
{
int i,j;
int touchsensor_value,Result1,mask;
int touchsensor,buzzer;



for (int j=0; j<15;j++) 

//while (1)
{

if(j%3==1)
			touchsensor_value = 1;
	else
			touchsensor_value =0;
			


			
//  asm code to read sensor value

	asm volatile(
		"andi %0, x30, 0x01\n\t"
		: "=r" (touchsensor_value)
		:
		: 
		);



//if condition logic
if (touchsensor_value)
	{
	mask=0xFFFFFFFD;
	
	// printf(" \n");
	
	buzzer=1;
	
	asm volatile(    
            "ori x30, x30,2"               
            :
            :
            :"x30"
            );
            
            asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(Result1)
	    	:
	    	:"x30"
	    	);
    	printf("Result1 = %d\n",Result1);
    	
	

	}
else
	{
	
	mask=0xFFFFFFFD;
	
	buzzer=0;
	
	
	asm volatile( 
            "and x30,x30, %0\n\t"     
            "ori x30, x30,0"            
            :
            :"r"(mask)
            :"x30"
        );
        asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(Result1)
	    	:
	    	:"x30"
	    	);
	 printf("Result1 = %d\n",Result1);

	}
	printf("buzzer=%d \n", touchsensor_value); 
}

return 0;
}
```
## The simulation commands and outputs are as follows:

```bash
riscv64-unknown-elf-gcc -march=rv64i -mabi=lp64 -ffreestanding -o out file.c
spike pk out
```

## Spike Output:

![Screenshot from 2023-10-25 15-30-35](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/fc6caa21-cd7f-4b02-8634-2a5a4402cd9e)


## Functional Simulation:

![Screenshot from 2023-10-28 21-21-57](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/ccc73c28-fb7d-4b5f-a710-358395601a54)

![Screenshot from 2023-10-28 21-20-01](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/a6ed6763-bfe4-471c-b353-0e17c25e119d)
![Screenshot from 2023-10-31 16-30-42](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/2450bc3d-e889-4e23-aaa9-af93bbab9fd8)

## Gate Level Simulation:

The gate-level synthesis (GLS) process involves converting a behavioral or RTL (Register Transfer Level) description of a digital design into a gate-level netlist using a library of standard cells. This netlist represents the design using specific gates such as AND, OR, NOT, and flip-flops, as defined by the technology library being used.

Here are the steps involved in the gate-level synthesis process and the subsequent gate-level simulation using GTKWave:

1. Reading the Library:

```bash
read_liberty -lib sky130_fd_sc_hd__tt_025C_1v80_256.lib
```

This command reads the technology library that includes the information about the standard cells available in the target technology.

2. Reading the Verilog Files:
```bash
read_verilog processor.v
```
This command reads the Verilog files that describe the processor's design.

3. Synthesis:
```bash
synth -top wrapper
```
This command initiates the synthesis process, generating a gate-level netlist based on the specified top-level module.

4. Mapping Flip-Flops:

```bash
dfflibmap -liberty sky130_fd_sc_hd__tt_025C_1v80_256.lib
```
This command maps the flip-flops in the design to specific D flip-flops from the technology library.

5. ABC (Advanced Boolean Calculator) Synthesis:

```bash
abc -liberty sky130_fd_sc_hd__tt_025C_1v80_256.lib
```

This command performs technology mapping and optimization of the synthesized netlist.

6. Writing the Gate-Level Verilog Netlist:

```bash
write_verilog <filename.v>
```
This command writes the gate-level Verilog netlist to a specified file.

7. Gate-Level Simulation:

```bash
iverilog -o test testbench.v glstest.v sky130_sram_1kbyte_1rw1r_32x256_8.v sky130_fd_sc_hd.v primitives.v
```

This command compiles the Verilog testbench and design files along with the necessary libraries and primitive modules.

8. Viewing Waveforms:

The resulting simulation is then visualized using GTKWave, which allows you to view the waveforms of signals in the design to verify the functionality of the synthesized netlist.

![Screenshot from 2023-11-02 21-59-37](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/ab5848b5-a446-4953-bfba-b71b52c2c983)
![Screenshot from 2023-11-02 22-00-07](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/b2dfdbcc-982b-4e8f-9f52-82739a3657c6)

