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
// Function to monitor water level and control the buzzer
//#include<stdio.h>
int main()  {

    int touchsensor_value;
    int buzzer;
    int mask;
    int i;
    int touchsensor;
    int test,test1;
    
    
        //for (int j=0; j<15;j++) {
       while(1){
       /* 
	
       if(j<10)
			touchsensor_value=1;
	else
			touchsensor_value=0;
	*/		

		asm volatile(
		
		"andi %0, x30, 0x01\n\t"
		: "=r" (touchsensor_value)
		: 
		:
		);
	

 
       
        if (touchsensor_value == 1) {
            // If moisture is below the threshold, set the buzzer control register to 1 (buzzer on)
            mask=0xFFFFFFFD;
          //  printf("water \n");
          buzzer = 1; 
       
            asm volatile(
            "and x30,x30, %0\n\t"     // Load immediate 1 into x30
            "ori x30, x30,2"                 // output at 2nd bit , that switches on the buzzer
            :
            :"r"(mask)
            :"x30"
            );
            asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(test)
	    	:
	    	:"x30"
	    	);
    	//printf("Test = %d\n",test);
            
      
        } 
        else {
            // If moisture is above the threshold, set the buzzer control register to 0 (buzzer off)
            mask=0xFFFFFFFD;
             buzzer = 0; 
          //  printf("no watre \n ");
            asm volatile( 
            "and x30,x30, %0\n\t"     // Load immediate 1 into x30
            "ori x30, x30,0"            //// output at 2nd bit , that switches off the buzzer
            :
            :"r"(mask)
            :"x30"
        );
        asm volatile(
	    	"addi %0, x30, 0\n\t"
	    	:"=r"(test1)
	    	:
	    	:"x30"
	    	);
	//printf("Test = %d\n",test1);
        }

  //printf("buzzer=%d \n", buzzer);   
}


return 0;
}

```

## Assembly Code
```assembly

out:     file format elf32-littleriscv


Disassembly of section .text:

00010054 <main>:
   10054:	fd010113          	addi	sp,sp,-48
   10058:	02812623          	sw	s0,44(sp)
   1005c:	03010413          	addi	s0,sp,48
   10060:	001f7793          	andi	a5,t5,1
   10064:	fef42623          	sw	a5,-20(s0)
   10068:	fec42703          	lw	a4,-20(s0)
   1006c:	00100793          	li	a5,1
   10070:	02f71663          	bne	a4,a5,1009c <main+0x48>
   10074:	ffd00793          	li	a5,-3
   10078:	fef42423          	sw	a5,-24(s0)
   1007c:	00100793          	li	a5,1
   10080:	fef42223          	sw	a5,-28(s0)
   10084:	fe842783          	lw	a5,-24(s0)
   10088:	00ff7f33          	and	t5,t5,a5
   1008c:	002f6f13          	ori	t5,t5,2
   10090:	000f0793          	mv	a5,t5
   10094:	fef42023          	sw	a5,-32(s0)
   10098:	fc9ff06f          	j	10060 <main+0xc>
   1009c:	ffd00793          	li	a5,-3
   100a0:	fef42423          	sw	a5,-24(s0)
   100a4:	fe042223          	sw	zero,-28(s0)
   100a8:	fe842783          	lw	a5,-24(s0)
   100ac:	00ff7f33          	and	t5,t5,a5
   100b0:	000f6f13          	ori	t5,t5,0
   100b4:	000f0793          	mv	a5,t5
   100b8:	fcf42e23          	sw	a5,-36(s0)
   100bc:	fa5ff06f          	j	10060 <main+0xc>
```

## RISCV Instruction in Assembly Code

```
Number of different instructions: 10
List of unique instructions:
sw
and
lw
andi
addi
li
mv
bne
ori
j
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

![Screenshot from 2023-11-06 10-57-08](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/77120591-5ce3-4e5e-be9e-7ecbd4c8a09e)

# wrapper module after netlist created
```bash
show wrapper
```

![Screenshot from 2023-11-06 11-49-38](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/83e5c3f6-2db5-452c-a54a-3cb35e4eda9f)

# Preparing the Design:

Preparing the design and including the lef files: The commands to prepare the design and overwite in a existing run folder the reports and results along with the command to include the lef files is given below:

```bash
# Update library files
sed -i 's/max_transition :0.04/max_transition :0.75/' *.lib

# Mount directories
make mount

# Run OpenLANE interactively
./flow.tcl -interactive

# Check OpenLANE version
% package require openlane 0.9

# Prepare design
% prep -design project -verbose 99
```
![Screenshot from 2023-11-15 22-55-49](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/b9a1c588-a1a3-4af6-92a5-45049fc282d8)

## Synthesis

Logic synthesis transforms the RTL netlist through two key steps:

- GTECH Mapping:
    - Maps the HDL netlist to generic gates.
    - Enables logical optimization using AIGERs and other topologies derived from the generic mapped netlist.

- Technology Mapping:
    - Maps the post-optimized GTECH netlist to standard cells specified in the PDK.

To initiate synthesis, use:

```bash
run_synthesis
```

![Screenshot from 2023-11-15 23-02-02](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/eaa78027-b9c0-4e5f-9501-f4b15f402bf6)

## Floorplan:

The floorplan stage aims to strategically organize the silicon area, establish a reliable power distribution network (PDN), and define macro placement and blockages for a legalized GDS file. Key considerations include achieving a balanced core utilization, setting aspect ratios, and incorporating power planning features.

Environment Variables / Switches:

    `FP_CORE_UTIL:` Specifies floorplan core utilization.
    `FP_ASPECT_RATIO:` Defines the floorplan aspect ratio.
    `FP_CORE_MARGIN:` Sets the core-to-die margin area.
    `FP_IO_MODE:` Configures pin arrangements (1 for equidistant, 0 for non-equidistant).
    `FP_CORE_VMETAL:` Specifies the vertical metal layer for the core.
    `FP_CORE_HMETAL:` Specifies the horizontal metal layer for the core.

Note: Metal layer values are typically 1 more than those specified.

Power Planning:

    Ring Formation: Connects to pads, enabling power distribution along the chip's edges.
    Power Straps: Utilizes higher metal layers to deliver power to the chip's middle, reducing IR drop and addressing electro-migration issues.

Floorplan Execution:
To execute the floorplan, use the following command:

```bash

% run_floorplan
```

This command initiates the floorplanning process, incorporating the specified environment variables and switches. Proper configuration ensures optimal silicon utilization, efficient power distribution, and a layout conducive to subsequent design stages.

![Screenshot from 2023-11-15 23-07-23](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/4fb43e7b-2386-46fe-8483-f5591cb86f38)


- Post the floorplan run, a .def file will have been created within the results/floorplan directory. We may review floorplan files by checking the floorplan.tcl.
- To view the floorplan: Magic is invoked after moving to the results/floorplan directory,then use the floowing command:
- 
```bash
magic -T /home/akhilasati/vsdstdcelldesign/libs/sky130A.tech lef read /home/OpenLane/designs/touch_sensor/runs/RUN_2023.11.14_08.46.33/tmp merged.nom.lef def read wrapper.def &
```
![Screenshot from 2023-11-15 23-57-12](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/ac94b061-c3ab-4336-ade9-d48330f644b3)


Die area (after floorplan)

![Screenshot from 2023-11-16 00-50-27](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/c2ced502-9a72-4d87-8d6f-7e4e1ebfe592)


Core area (after floorplan)

![Screenshot from 2023-11-16 00-50-18](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/83ea8a39-2b2c-4f52-ac4d-d8ebe3e6666b)

## Placement Overview:

The placement stage in the OpenLANE ASIC flow involves arranging standard cells on floorplan rows, aligning them with sites specified in the technology LEF file. Placement is executed in two stages: Global and Detailed, aiming to optimize cell positions and ensure legality.

    - Global Placement:
        `Objective:` Finds optimal positions for all cells, allowing potential overlap and disregarding alignment to rows.
        `Optimization:` Focuses on reducing half parameter wire length to enhance overall performance.
        `Result:` Provides a preliminary arrangement, optimizing for wire length but not necessarily adhering to legal placement constraints.

    - Detailed Placement:
        `Objective:` Refines cell positions post-global placement to legalize and align them with floorplan rows.
        `Optimization:` Adjusts cell positions while respecting global placement constraints.
        `Result:` Yields a legally placed layout that aligns with the floorplan and satisfies design rules.

### Placement Execution:
To run the placement process, execute the following command:

```bash
run_placement
```

This command initiates both the Global and Detailed Placement stages, progressing the design towards a physically realizable layout. Proper placement is crucial for meeting performance and design rule specifications in the subsequent steps of the ASIC flow.

![Screenshot from 2023-11-16 00-55-08](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/209a6ede-3e95-48b0-94bb-e9257e4069ef)

Post placement: the design can be viewed on magic within results/placement directory. Run the follwing command in that directory:

```bash
magic -T /home/akhilasati/vsdstdcelldesign/libs/sky130A.tech lef read /home/OpenLane/designs/touch_sensor/runs/RUN_2023.11.14_08.46.33/tmp merged.nom.lef def read wrapper.def &
```

![Screenshot from 2023-11-16 00-57-02](https://github.com/akhiiasati/IIITB_Advanced_Physical_Design_using_OpenLANE_Sky130/assets/43675821/29a262a9-d3f6-4be5-bda1-38e65f6888fe)


