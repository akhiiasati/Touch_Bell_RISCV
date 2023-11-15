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


![WhatsApp Image 2023-11-16 at 02 31 43_05327e13](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/133416c4-aee6-4494-a31e-1bb281e4810f)


## Clock Tree Synthesis (CTS) Overview:

Clock Tree Synthesis is a critical step in the ASIC design flow aimed at creating an efficient clock distribution network for delivering the clock signal to all sequential elements. The primary objective is to minimize clock skew across the entire chip, ensuring synchronous operation of the design. H-trees are commonly employed as a network topology to achieve this goal.

### Key Objectives:

- Clock Distribution: Ensures the clock signal reaches every sequential element in the design.
- Clock Skew Minimization: Aims to achieve zero clock skew across the chip.
- H-tree Topology: Common methodology in CTS, providing an effective structure for distributing the clock signal evenly.

#### Command to Run CTS:
To perform Clock Tree Synthesis, execute the following command:

```bash
run_cts
```

This command triggers the CTS process, where the tool generates an optimized clock tree structure based on the design requirements and constraints. Achieving a balanced clock distribution is crucial for maintaining synchronization and meeting performance criteria in subsequent stages of the ASIC design flow.

![Screenshot from 2023-11-16 01-04-38](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/8f90dab2-1f53-47f1-8974-f546983bc747)

### Timing Reports

![Screenshot from 2023-11-16 01-32-14](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/72960463-2357-4155-a206-28e7991503dc)


### AREA Reports

![Screenshot from 2023-11-16 01-32-32](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/21c8d751-ba10-4ebf-86a1-b8ecdf2e7c80)


### Skew Reports

![Screenshot from 2023-11-16 01-33-04](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/6b0c12ec-a203-4af0-9a58-8d61eb06d841)


### Power Reports

![Screenshot from 2023-11-16 01-33-17](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/ef016302-b176-488b-bc7c-51cbed1fff13)

# Routing:

Routing in the OpenLANE ASIC flow involves implementing the interconnect system between standard cells using the remaining available metal layers post Clock Tree Synthesis (CTS) and Power Distribution Network (PDN) generation. TritonRoute is the tool used for routing in OpenLANE, and the process is divided into two stages: Global Routing and Detailed Routing.

### Global Routing:
`Routing Grids:` The routing region is divided into rectangular grids, represented as coarse 3D routes using the Fastroute tool.
`Objective:` Establishes a high-level routing plan across the chip.
`Result:` Provides a global routing framework to guide the subsequent detailed routing stage.

### Detailed Routing:
`Routing Grids and Guides:` Utilizes finer grids and routing guides to implement the physical wiring, employing the TritonRoute tool.
`Features of TritonRoute:`
- Honors pre-processed route guides.
- Assumes that each net satisfies inter-guide connectivity.'
- Utilizes a Mixed-Integer Linear Programming (MILP) based panel routing scheme.
- Implements an intra-layer parallel and inter-layer sequential routing framework.

### Running the Routing Process:
To execute the routing process, use the following command:

```bash
% run_routing
```

This command initiates both the Global Routing and Detailed Routing stages, resulting in a well-defined interconnect system that adheres to design rules and minimizes design rule check (DRC) errors. Proper routing is essential for ensuring signal integrity and meeting performance requirements in the final chip design.

![Screenshot from 2023-11-16 01-41-08](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/25dbec4b-f530-4750-8f1e-676023800b14)

![WhatsApp Image 2023-11-16 at 02 31 43_a72b96b9](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/3bd17745-d375-4c22-ba3c-4b16a99bbec8)


View the post-routing design in Magic:

```bash
% magic -T /home/akhilasati/vsdstdcelldesign/libs/sky130A.tech lef read /home/OpenLane/designs/touch_sensor/runs/RUN_2023.11.14_08.46.33/tmp merged.nom.lef def read wrapper.def &

```

![Screenshot from 2023-11-16 01-51-58](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/c0f30791-159a-4d6d-93b8-c7e785ef0705)


#### post_routing Timing Reports

![Screenshot from 2023-11-16 01-56-31](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/46e10986-80ef-4424-8e0b-9cda7c3d9e97)


#### post_routing Area Reports

![Screenshot from 2023-11-16 01-53-21](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/16635e3b-4798-4593-8803-f355ea689f18)

#### post_routing Power Reports

![Screenshot from 2023-11-16 01-57-19](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/8475ba77-ac4f-448f-abd4-694bf9e854ec)


Here DRC violation is zero:
![Screenshot from 2023-11-16 02-01-48](https://github.com/akhiiasati/Touch_Bell_RISCV/assets/43675821/6654f7e2-edfd-455a-aac3-2ba5872ca7ee)

# Word of Thanks
I sciencerly thank Mr. Kunal Gosh(Founder/VSD) for helping me out to complete this flow smoothly.

# Acknowledgement
- Kunal Ghosh, VSD Corp. Pvt. Ltd.
- Skywater Foundry
- Mayank Kabra,Founder,Chipcron Pvt.Ltd.

# Reference
- https://github.com/The-OpenROAD-Project/OpenSTA.git
- https://github.com/kunalg123
- https://www.vsdiat.com
- https://github.com/SakethGajawada/RISCV-GNU

