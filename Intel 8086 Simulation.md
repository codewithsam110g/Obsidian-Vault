## Specifications:
1. 20 bit bus, segmented using multiple registers like CS + IP, SS + IP, DS + IP etc
2. each register / pointer is 16 bits so 2 bytes each
3. 2 separate processing units: BIU ( Bus Information Unit ) and EU (Execution Unit)
4. 256 Interupts
5. Registers:
	1. General Purpose
		1. Accumulator Register, AX
		2. Base Register, BX
		3. Count Register, CX
		4. Data Register, DX
	2. Segment Registers
		1. Code Segment, CS
		2. Data Segment, DS
		3. Stack Segment, SS
		4. Extra Segment, ES
	3. Pointers and Index Registers
		1. Instruction Pointer, IP
		2. Stack Pointer, SP
		3. Base Pointer, BP
		4. Source Index, SI
		5. Destination Index, DI
	4. Flags Register
		1. Status Register, this contains all the flags like

| Bit Position | Flag Name                     | Description                                                                    |
| ------------ | ----------------------------- | ------------------------------------------------------------------------------ |
| 0            | **CF** (Carry Flag)           | Indicates a carry or borrow in arithmetic operations.                          |
| 1            | **Reserved**                  | Reserved for future use; not utilized.                                         |
| 2            | **PF** (Parity Flag)          | Set if the number of set bits in the result is even.                           |
| 3            | **Reserved**                  | Reserved for future use; not utilized.                                         |
| 4            | **AF** (Auxiliary Carry Flag) | Set if there is a carry from the lower nibble (4 bits).                        |
| 5            | **Reserved**                  | Reserved for future use; not utilized.                                         |
| 6            | **ZF** (Zero Flag)            | Set if the result of an operation is zero.                                     |
| 7            | **SF** (Sign Flag)            | Set if the result is negative (most significant bit is 1).                     |
| 8            | **TF** (Trap Flag)            | Used for single-step debugging; generates an exception after each instruction. |
| 9            | **IF** (Interrupt Flag)       | Controls the response to external interrupts.                                  |
| 10           | **DF** (Direction Flag)       | Controls the direction of string operations.                                   |
| 11           | **Reserved**                  | Reserved for future use; not utilized.                                         |
| 12           | **OF** (Overflow Flag)        | Set if an arithmetic operation results in an overflow.                         |
| 13           | **Reserved**                  | Reserved for future use; not utilized.                                         |
| 14           | **Reserved**                  | Reserved for future use; not utilized.                                         |
| 15           | **Reserved**                  | Reserved for future use; not utilized.                                         |

## Interupts
1. There are 256 interupts in 8086, size = 256 \* 4 bytes = 1 KB
2. 2 bytes for SEGMENT and 2 bytes for OFFSET for full 20-bit address
3. When interupt is triggered it saves the current state and looks for the seg and off pointers to load
4. after the interupt is handled it reloads the old / previous state and continues the previous task
5. Interupts are of 2 types: software and hardware
6. Software interupts are interupts in code, triggering things like read and write to file, display etc
7. Hardware interupts are physical signals from the peripherals like mouse, keyboard etc to the cpu
## Interupt Setup & Booting
1. When the device is powered on, the 8086 is starts at the reset vector
2. the reset vector points to the mapped memory space address of the bios to ram
3. then it cpu loads and start executing the bios,
4. the bios sets up the basic interupts and looks for the storage devices with the boot enabled signal (usually 0x55ff at the end of the first sector).
5. then it gives the access to that first sector which contains the os kernel and it loads the drivers, required files for the os and loads the os, where it wll specify the it own functions for the interupts and updates the new function pointers in the ivt 
## Hardware Interupts
1. Apart from the software interupts which are triggered in code, a Hardware Interupt is a physical interupt signal
2. Then the whole INTA cycle takes place
## INTA Cycle
1. Lets look at INTA cycle with Keypress Example
2. Keypress detected, it sends the scan code to the keyboard controller
3. the kb controller send an irq to pic like irq1
4. if no higher irq is present, the pic send the int to the cpu
5. the cpu finishes the current instruction and saves the current state (cs, ip,flags  and registers).
6. it then send a ack to the pic, which moves the irq to being processed state (register inside pic) and sends the correct ivt value to the cpu
7. while this is done the kb controller sees that the irq is in process and sends the data (scan code) to the device mapped memory in the ram
8. the cpu then uses the ivt index value to find the isr cs and ip value in the ivt to load that specific isr
9. then the isr is processed and cpu calls the iret and send eoi to the pic
10. then cpu restores the state saved before processing the interupt

## Memory Map
1. First 640 KB are for OS and Software
2. Then from 0xA000H to 0xFFFFH is for Video Memory 128 KB
3. then there is 0xC000H to 0xEFFFH is for Memory mapped Devices 192 KB
4. last 0xF000H to 0xFFFFH is for BIOS / ROM 64 KB