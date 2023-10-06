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
            asm(
		"andi %0, x30, 1\n\t"
		:"=r"(ledPin));
            // Use printf to indicate "TOUCHED" without Serial
            //printf("TOUCHED\n");
        } else {
            ledPin=0;
             asm(
		"andi %0, x30, 1\n\t"
		:"=r"(ledPin));
            // Use printf to indicate "not touched" without Serial
            //printf("not touched\n");
        }
        delay(500); // Delay for 500 milliseconds (0.5 seconds)
    }

    return 0;
}
```
## Assembly Code
```assembly

touchsensor:     file format elf32-littleriscv


Disassembly of section .text:

00010094 <exit>:
   10094:	ff010113          	add	sp,sp,-16
   10098:	00000593          	li	a1,0
   1009c:	00812423          	sw	s0,8(sp)
   100a0:	00112623          	sw	ra,12(sp)
   100a4:	00050413          	mv	s0,a0
   100a8:	2ac000ef          	jal	10354 <__call_exitprocs>
   100ac:	c281a503          	lw	a0,-984(gp) # 119f0 <_global_impure_ptr>
   100b0:	03c52783          	lw	a5,60(a0)
   100b4:	00078463          	beqz	a5,100bc <exit+0x28>
   100b8:	000780e7          	jalr	a5
   100bc:	00040513          	mv	a0,s0
   100c0:	4bc000ef          	jal	1057c <_exit>

000100c4 <register_fini>:
   100c4:	00000793          	li	a5,0
   100c8:	00078863          	beqz	a5,100d8 <register_fini+0x14>
   100cc:	00010537          	lui	a0,0x10
   100d0:	47450513          	add	a0,a0,1140 # 10474 <__libc_fini_array>
   100d4:	3fc0006f          	j	104d0 <atexit>
   100d8:	00008067          	ret

000100dc <_start>:
   100dc:	00002197          	auipc	gp,0x2
   100e0:	cec18193          	add	gp,gp,-788 # 11dc8 <__global_pointer$>
   100e4:	c3418513          	add	a0,gp,-972 # 119fc <completed.1>
   100e8:	c5018613          	add	a2,gp,-944 # 11a18 <__BSS_END__>
   100ec:	40a60633          	sub	a2,a2,a0
   100f0:	00000593          	li	a1,0
   100f4:	184000ef          	jal	10278 <memset>
   100f8:	00000517          	auipc	a0,0x0
   100fc:	3d850513          	add	a0,a0,984 # 104d0 <atexit>
   10100:	00050863          	beqz	a0,10110 <_start+0x34>
   10104:	00000517          	auipc	a0,0x0
   10108:	37050513          	add	a0,a0,880 # 10474 <__libc_fini_array>
   1010c:	3c4000ef          	jal	104d0 <atexit>
   10110:	0cc000ef          	jal	101dc <__libc_init_array>
   10114:	00012503          	lw	a0,0(sp)
   10118:	00410593          	add	a1,sp,4
   1011c:	00000613          	li	a2,0
   10120:	06c000ef          	jal	1018c <main>
   10124:	f71ff06f          	j	10094 <exit>

00010128 <__do_global_dtors_aux>:
   10128:	ff010113          	add	sp,sp,-16
   1012c:	00812423          	sw	s0,8(sp)
   10130:	c341c783          	lbu	a5,-972(gp) # 119fc <completed.1>
   10134:	00112623          	sw	ra,12(sp)
   10138:	02079263          	bnez	a5,1015c <__do_global_dtors_aux+0x34>
   1013c:	00000793          	li	a5,0
   10140:	00078a63          	beqz	a5,10154 <__do_global_dtors_aux+0x2c>
   10144:	00011537          	lui	a0,0x11
   10148:	5b450513          	add	a0,a0,1460 # 115b4 <__EH_FRAME_BEGIN__>
   1014c:	00000097          	auipc	ra,0x0
   10150:	000000e7          	jalr	zero # 0 <exit-0x10094>
   10154:	00100793          	li	a5,1
   10158:	c2f18a23          	sb	a5,-972(gp) # 119fc <completed.1>
   1015c:	00c12083          	lw	ra,12(sp)
   10160:	00812403          	lw	s0,8(sp)
   10164:	01010113          	add	sp,sp,16
   10168:	00008067          	ret

0001016c <frame_dummy>:
   1016c:	00000793          	li	a5,0
   10170:	00078c63          	beqz	a5,10188 <frame_dummy+0x1c>
   10174:	00011537          	lui	a0,0x11
   10178:	c3818593          	add	a1,gp,-968 # 11a00 <object.0>
   1017c:	5b450513          	add	a0,a0,1460 # 115b4 <__EH_FRAME_BEGIN__>
   10180:	00000317          	auipc	t1,0x0
   10184:	00000067          	jr	zero # 0 <exit-0x10094>
   10188:	00008067          	ret

0001018c <main>:
   1018c:	fe010113          	add	sp,sp,-32
   10190:	00812e23          	sw	s0,28(sp)
   10194:	02010413          	add	s0,sp,32
   10198:	fe042623          	sw	zero,-20(s0)
   1019c:	fe042423          	sw	zero,-24(s0)
   101a0:	00ff6f33          	or	t5,t5,a5
   101a4:	fef42623          	sw	a5,-20(s0)
   101a8:	001f7793          	and	a5,t5,1
   101ac:	fef42423          	sw	a5,-24(s0)
   101b0:	fec42783          	lw	a5,-20(s0)
   101b4:	00078c63          	beqz	a5,101cc <main+0x40>
   101b8:	00100793          	li	a5,1
   101bc:	fef42423          	sw	a5,-24(s0)
   101c0:	001f7793          	and	a5,t5,1
   101c4:	fef42423          	sw	a5,-24(s0)
   101c8:	fe9ff06f          	j	101b0 <main+0x24>
   101cc:	fe042423          	sw	zero,-24(s0)
   101d0:	001f7793          	and	a5,t5,1
   101d4:	fef42423          	sw	a5,-24(s0)
   101d8:	fd9ff06f          	j	101b0 <main+0x24>

000101dc <__libc_init_array>:
   101dc:	ff010113          	add	sp,sp,-16
   101e0:	00812423          	sw	s0,8(sp)
   101e4:	000117b7          	lui	a5,0x11
   101e8:	00011437          	lui	s0,0x11
   101ec:	01212023          	sw	s2,0(sp)
   101f0:	5b878793          	add	a5,a5,1464 # 115b8 <__init_array_start>
   101f4:	5b840713          	add	a4,s0,1464 # 115b8 <__init_array_start>
   101f8:	00112623          	sw	ra,12(sp)
   101fc:	00912223          	sw	s1,4(sp)
   10200:	40e78933          	sub	s2,a5,a4
   10204:	02e78263          	beq	a5,a4,10228 <__libc_init_array+0x4c>
   10208:	40295913          	sra	s2,s2,0x2
   1020c:	5b840413          	add	s0,s0,1464
   10210:	00000493          	li	s1,0
   10214:	00042783          	lw	a5,0(s0)
   10218:	00148493          	add	s1,s1,1
   1021c:	00440413          	add	s0,s0,4
   10220:	000780e7          	jalr	a5
   10224:	ff24e8e3          	bltu	s1,s2,10214 <__libc_init_array+0x38>
   10228:	00011437          	lui	s0,0x11
   1022c:	000117b7          	lui	a5,0x11
   10230:	5c078793          	add	a5,a5,1472 # 115c0 <__do_global_dtors_aux_fini_array_entry>
   10234:	5b840713          	add	a4,s0,1464 # 115b8 <__init_array_start>
   10238:	40e78933          	sub	s2,a5,a4
   1023c:	40295913          	sra	s2,s2,0x2
   10240:	02e78063          	beq	a5,a4,10260 <__libc_init_array+0x84>
   10244:	5b840413          	add	s0,s0,1464
   10248:	00000493          	li	s1,0
   1024c:	00042783          	lw	a5,0(s0)
   10250:	00148493          	add	s1,s1,1
   10254:	00440413          	add	s0,s0,4
   10258:	000780e7          	jalr	a5
   1025c:	ff24e8e3          	bltu	s1,s2,1024c <__libc_init_array+0x70>
   10260:	00c12083          	lw	ra,12(sp)
   10264:	00812403          	lw	s0,8(sp)
   10268:	00412483          	lw	s1,4(sp)
   1026c:	00012903          	lw	s2,0(sp)
   10270:	01010113          	add	sp,sp,16
   10274:	00008067          	ret

00010278 <memset>:
   10278:	00f00313          	li	t1,15
   1027c:	00050713          	mv	a4,a0
   10280:	02c37e63          	bgeu	t1,a2,102bc <memset+0x44>
   10284:	00f77793          	and	a5,a4,15
   10288:	0a079063          	bnez	a5,10328 <memset+0xb0>
   1028c:	08059263          	bnez	a1,10310 <memset+0x98>
   10290:	ff067693          	and	a3,a2,-16
   10294:	00f67613          	and	a2,a2,15
   10298:	00e686b3          	add	a3,a3,a4
   1029c:	00b72023          	sw	a1,0(a4)
   102a0:	00b72223          	sw	a1,4(a4)
   102a4:	00b72423          	sw	a1,8(a4)
   102a8:	00b72623          	sw	a1,12(a4)
   102ac:	01070713          	add	a4,a4,16
   102b0:	fed766e3          	bltu	a4,a3,1029c <memset+0x24>
   102b4:	00061463          	bnez	a2,102bc <memset+0x44>
   102b8:	00008067          	ret
   102bc:	40c306b3          	sub	a3,t1,a2
   102c0:	00269693          	sll	a3,a3,0x2
   102c4:	00000297          	auipc	t0,0x0
   102c8:	005686b3          	add	a3,a3,t0
   102cc:	00c68067          	jr	12(a3)
   102d0:	00b70723          	sb	a1,14(a4)
   102d4:	00b706a3          	sb	a1,13(a4)
   102d8:	00b70623          	sb	a1,12(a4)
   102dc:	00b705a3          	sb	a1,11(a4)
   102e0:	00b70523          	sb	a1,10(a4)
   102e4:	00b704a3          	sb	a1,9(a4)
   102e8:	00b70423          	sb	a1,8(a4)
   102ec:	00b703a3          	sb	a1,7(a4)
   102f0:	00b70323          	sb	a1,6(a4)
   102f4:	00b702a3          	sb	a1,5(a4)
   102f8:	00b70223          	sb	a1,4(a4)
   102fc:	00b701a3          	sb	a1,3(a4)
   10300:	00b70123          	sb	a1,2(a4)
   10304:	00b700a3          	sb	a1,1(a4)
   10308:	00b70023          	sb	a1,0(a4)
   1030c:	00008067          	ret
   10310:	0ff5f593          	zext.b	a1,a1
   10314:	00859693          	sll	a3,a1,0x8
   10318:	00d5e5b3          	or	a1,a1,a3
   1031c:	01059693          	sll	a3,a1,0x10
   10320:	00d5e5b3          	or	a1,a1,a3
   10324:	f6dff06f          	j	10290 <memset+0x18>
   10328:	00279693          	sll	a3,a5,0x2
   1032c:	00000297          	auipc	t0,0x0
   10330:	005686b3          	add	a3,a3,t0
   10334:	00008293          	mv	t0,ra
   10338:	fa0680e7          	jalr	-96(a3)
   1033c:	00028093          	mv	ra,t0
   10340:	ff078793          	add	a5,a5,-16
   10344:	40f70733          	sub	a4,a4,a5
   10348:	00f60633          	add	a2,a2,a5
   1034c:	f6c378e3          	bgeu	t1,a2,102bc <memset+0x44>
   10350:	f3dff06f          	j	1028c <memset+0x14>

00010354 <__call_exitprocs>:
   10354:	fd010113          	add	sp,sp,-48
   10358:	01412c23          	sw	s4,24(sp)
   1035c:	c281aa03          	lw	s4,-984(gp) # 119f0 <_global_impure_ptr>
   10360:	03212023          	sw	s2,32(sp)
   10364:	02112623          	sw	ra,44(sp)
   10368:	148a2903          	lw	s2,328(s4)
   1036c:	02812423          	sw	s0,40(sp)
   10370:	02912223          	sw	s1,36(sp)
   10374:	01312e23          	sw	s3,28(sp)
   10378:	01512a23          	sw	s5,20(sp)
   1037c:	01612823          	sw	s6,16(sp)
   10380:	01712623          	sw	s7,12(sp)
   10384:	01812423          	sw	s8,8(sp)
   10388:	04090063          	beqz	s2,103c8 <__call_exitprocs+0x74>
   1038c:	00050b13          	mv	s6,a0
   10390:	00058b93          	mv	s7,a1
   10394:	00100a93          	li	s5,1
   10398:	fff00993          	li	s3,-1
   1039c:	00492483          	lw	s1,4(s2)
   103a0:	fff48413          	add	s0,s1,-1
   103a4:	02044263          	bltz	s0,103c8 <__call_exitprocs+0x74>
   103a8:	00249493          	sll	s1,s1,0x2
   103ac:	009904b3          	add	s1,s2,s1
   103b0:	040b8463          	beqz	s7,103f8 <__call_exitprocs+0xa4>
   103b4:	1044a783          	lw	a5,260(s1)
   103b8:	05778063          	beq	a5,s7,103f8 <__call_exitprocs+0xa4>
   103bc:	fff40413          	add	s0,s0,-1
   103c0:	ffc48493          	add	s1,s1,-4
   103c4:	ff3416e3          	bne	s0,s3,103b0 <__call_exitprocs+0x5c>
   103c8:	02c12083          	lw	ra,44(sp)
   103cc:	02812403          	lw	s0,40(sp)
   103d0:	02412483          	lw	s1,36(sp)
   103d4:	02012903          	lw	s2,32(sp)
   103d8:	01c12983          	lw	s3,28(sp)
   103dc:	01812a03          	lw	s4,24(sp)
   103e0:	01412a83          	lw	s5,20(sp)
   103e4:	01012b03          	lw	s6,16(sp)
   103e8:	00c12b83          	lw	s7,12(sp)
   103ec:	00812c03          	lw	s8,8(sp)
   103f0:	03010113          	add	sp,sp,48
   103f4:	00008067          	ret
   103f8:	00492783          	lw	a5,4(s2)
   103fc:	0044a683          	lw	a3,4(s1)
   10400:	fff78793          	add	a5,a5,-1
   10404:	04878e63          	beq	a5,s0,10460 <__call_exitprocs+0x10c>
   10408:	0004a223          	sw	zero,4(s1)
   1040c:	fa0688e3          	beqz	a3,103bc <__call_exitprocs+0x68>
   10410:	18892783          	lw	a5,392(s2)
   10414:	008a9733          	sll	a4,s5,s0
   10418:	00492c03          	lw	s8,4(s2)
   1041c:	00f777b3          	and	a5,a4,a5
   10420:	02079263          	bnez	a5,10444 <__call_exitprocs+0xf0>
   10424:	000680e7          	jalr	a3
   10428:	00492703          	lw	a4,4(s2)
   1042c:	148a2783          	lw	a5,328(s4)
   10430:	01871463          	bne	a4,s8,10438 <__call_exitprocs+0xe4>
   10434:	f92784e3          	beq	a5,s2,103bc <__call_exitprocs+0x68>
   10438:	f80788e3          	beqz	a5,103c8 <__call_exitprocs+0x74>
   1043c:	00078913          	mv	s2,a5
   10440:	f5dff06f          	j	1039c <__call_exitprocs+0x48>
   10444:	18c92783          	lw	a5,396(s2)
   10448:	0844a583          	lw	a1,132(s1)
   1044c:	00f77733          	and	a4,a4,a5
   10450:	00071c63          	bnez	a4,10468 <__call_exitprocs+0x114>
   10454:	000b0513          	mv	a0,s6
   10458:	000680e7          	jalr	a3
   1045c:	fcdff06f          	j	10428 <__call_exitprocs+0xd4>
   10460:	00892223          	sw	s0,4(s2)
   10464:	fa9ff06f          	j	1040c <__call_exitprocs+0xb8>
   10468:	00058513          	mv	a0,a1
   1046c:	000680e7          	jalr	a3
   10470:	fb9ff06f          	j	10428 <__call_exitprocs+0xd4>

00010474 <__libc_fini_array>:
   10474:	ff010113          	add	sp,sp,-16
   10478:	00812423          	sw	s0,8(sp)
   1047c:	000117b7          	lui	a5,0x11
   10480:	00011437          	lui	s0,0x11
   10484:	5c078793          	add	a5,a5,1472 # 115c0 <__do_global_dtors_aux_fini_array_entry>
   10488:	5c440413          	add	s0,s0,1476 # 115c4 <__fini_array_end>
   1048c:	40f40433          	sub	s0,s0,a5
   10490:	00912223          	sw	s1,4(sp)
   10494:	00112623          	sw	ra,12(sp)
   10498:	40245493          	sra	s1,s0,0x2
   1049c:	02048063          	beqz	s1,104bc <__libc_fini_array+0x48>
   104a0:	ffc40413          	add	s0,s0,-4
   104a4:	00f40433          	add	s0,s0,a5
   104a8:	00042783          	lw	a5,0(s0)
   104ac:	fff48493          	add	s1,s1,-1
   104b0:	ffc40413          	add	s0,s0,-4
   104b4:	000780e7          	jalr	a5
   104b8:	fe0498e3          	bnez	s1,104a8 <__libc_fini_array+0x34>
   104bc:	00c12083          	lw	ra,12(sp)
   104c0:	00812403          	lw	s0,8(sp)
   104c4:	00412483          	lw	s1,4(sp)
   104c8:	01010113          	add	sp,sp,16
   104cc:	00008067          	ret

000104d0 <atexit>:
   104d0:	00050593          	mv	a1,a0
   104d4:	00000693          	li	a3,0
   104d8:	00000613          	li	a2,0
   104dc:	00000513          	li	a0,0
   104e0:	0040006f          	j	104e4 <__register_exitproc>

000104e4 <__register_exitproc>:
   104e4:	c281a703          	lw	a4,-984(gp) # 119f0 <_global_impure_ptr>
   104e8:	14872783          	lw	a5,328(a4)
   104ec:	04078c63          	beqz	a5,10544 <__register_exitproc+0x60>
   104f0:	0047a703          	lw	a4,4(a5)
   104f4:	01f00813          	li	a6,31
   104f8:	06e84e63          	blt	a6,a4,10574 <__register_exitproc+0x90>
   104fc:	00271813          	sll	a6,a4,0x2
   10500:	02050663          	beqz	a0,1052c <__register_exitproc+0x48>
   10504:	01078333          	add	t1,a5,a6
   10508:	08c32423          	sw	a2,136(t1) # 10208 <__libc_init_array+0x2c>
   1050c:	1887a883          	lw	a7,392(a5)
   10510:	00100613          	li	a2,1
   10514:	00e61633          	sll	a2,a2,a4
   10518:	00c8e8b3          	or	a7,a7,a2
   1051c:	1917a423          	sw	a7,392(a5)
   10520:	10d32423          	sw	a3,264(t1)
   10524:	00200693          	li	a3,2
   10528:	02d50463          	beq	a0,a3,10550 <__register_exitproc+0x6c>
   1052c:	00170713          	add	a4,a4,1
   10530:	00e7a223          	sw	a4,4(a5)
   10534:	010787b3          	add	a5,a5,a6
   10538:	00b7a423          	sw	a1,8(a5)
   1053c:	00000513          	li	a0,0
   10540:	00008067          	ret
   10544:	14c70793          	add	a5,a4,332
   10548:	14f72423          	sw	a5,328(a4)
   1054c:	fa5ff06f          	j	104f0 <__register_exitproc+0xc>
   10550:	18c7a683          	lw	a3,396(a5)
   10554:	00170713          	add	a4,a4,1
   10558:	00e7a223          	sw	a4,4(a5)
   1055c:	00c6e6b3          	or	a3,a3,a2
   10560:	18d7a623          	sw	a3,396(a5)
   10564:	010787b3          	add	a5,a5,a6
   10568:	00b7a423          	sw	a1,8(a5)
   1056c:	00000513          	li	a0,0
   10570:	00008067          	ret
   10574:	fff00513          	li	a0,-1
   10578:	00008067          	ret

0001057c <_exit>:
   1057c:	05d00893          	li	a7,93
   10580:	00000073          	ecall
   10584:	00054463          	bltz	a0,1058c <_exit+0x10>
   10588:	0000006f          	j	10588 <_exit+0xc>
   1058c:	ff010113          	add	sp,sp,-16
   10590:	00812423          	sw	s0,8(sp)
   10594:	00050413          	mv	s0,a0
   10598:	00112623          	sw	ra,12(sp)
   1059c:	40800433          	neg	s0,s0
   105a0:	00c000ef          	jal	105ac <__errno>
   105a4:	00852023          	sw	s0,0(a0)
   105a8:	0000006f          	j	105a8 <_exit+0x2c>

000105ac <__errno>:
   105ac:	c301a503          	lw	a0,-976(gp) # 119f8 <_impure_ptr>
   105b0:	00008067          	ret
```

## RISCV Instruction in Assembly Code

```
Number of different instructions: 30
List of unique instructions:
sw
sb
mv
auipc
li
bne
sll
bltu
and
neg
lui
jalr
sra
ret
jal
blt
jr
ecall
lw
bltz
zext.b
or
lbu
add
beq
j
bgeu
beqz
bnez
sub
```
