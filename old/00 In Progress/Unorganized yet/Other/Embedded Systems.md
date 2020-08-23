Embedded:
---------
- What is an embedded system?

Systems that contain one or more processing units, like a microcontroller doing specific things and respond to certain inputs.

They are dedicated to one function and perform it repeatedly.

They are part of a larger system "embedding system".

PC is not an embedded system because:
1- General-purpose processor.
2- Hardware is independent of software running on it.

- Necessary features of an embedded system.

1- Reliability: a failure may cause critical functions to not work, so it should not be prone to error.

2- Efficiency: it should act exactly at the time specified for the action. A delay or early action may result in critical functions not working.

That's why they are real-time system.

So, it should be SW efficient and HW efficient.

3- Tightly constrained: embedded systems are very small computers that have:
	i. Low processing speed.
	ii. Small memroy (for data).
	iii. Small memory (for code size).
	iv. Real-time (should be fast and efficient)
	v. predictable in execution time.
	vi. Power-limited i.e. in battery operated devices.
	vii. Cost
	viii. Size.
	
The developer should deal with all this, that's why they should be good software engineers.

Debugging tools should be closer to hardware e.g. lauterbach.

4. Single-functionality: embedded system that is dedicated to perform ONE function.

5. Complex-functionality: has to run complex or many algorithms e.g. cell phones, laser printers, etc.

6. Safety-critical: must be safe for humans.

7. Maintainable: it can be modified and enhanced.

8. Interactive: in order to be easily understandable, operated and handled by the user.

9. Strong association between HW and SW


------------------
So, embedded systems are:
- Limited in functionality.
- Not programmable by user.
- Fixed run-time requirements.
- Limited by criteria.

While general-purpose systems are:
- Not limited in functionality.
- Programmable by the user (install windows or whatever)
- Faster run-time requirements.
- Not limited by a lot of criteria.

-------------------
The costs of high-end cars come from the embedded systems in them.

Electronics are 40% of the total cost.

Examples:
- Autoparking.
- Vehicle communication.

Phones and tablets:
- Wifi, 3G, LTE, Bluetooth.
- Audio, video, and graphics processing.
- The application processor itself android, ios, windows phone.

Networking devices:
- Routers, switches, etc.
- Sattelites.

Robotics.
Industry.
Cameras.
Appliances.
Home automation.
Banking ATMs.
Aircraft control.
Homing missiles.
Wearables (google glass, smart watches).

----------------------
A)General purpose processor architecture:
- CPU = ALU + Registers + Control unit.

- 32/64-bit datapath.
- Advanced cache logic.
- Built-in math processor for FPU.
- Built-in memory management unit for memory protection and virtual memory.
- Interfaces to support external devices.
- Complex design to support a wide range of functionalities.
- Consume a LOT of power and produce a LOT of heat.

B)Embedded general-purpose processor:
- 16/32-bit datapath.
- Limited functionality according to application.
- Integrated into larger dedicated systems. E.g. ARM Cortex-M.
- Doesn't have memory, memory is external, but it has cache memory.

C)DSP:
- Focuses on very efficient execution of arithmetic ops since they perform them a LOT and must be quick.
- Used in phones, image & video processing, etc.
- Usually integrated with ADC and DAC.
----------------------
A) Microcontroller:
A computer on a chip:
- CPU.
- ROM.
- RAM.
- Timer.
- I/O unit.
- Interrupt handler.

Dedicated.

B) Microprocessor:
Just the CPU.

General-purpose.

> Microcontroller deployment options:
1. One chip has CPU and all peripherals.
2. Different chips on a PCB.
----------------------
Embedded system types:
1. System On Board SOB:
- Discrete units on one board.
- Used for dev and design.
- High power consumption.
- Configurable.

2. System On Chip SOC:
- Production.
- Unchangeable.
- One unit contains all stuff.
----------------------
Main components of ANY microcontroller:
- CPU.
- Memory.
- IO ports (GPIO or DIO)
- Timers
- Watchdog timer
- Interrupt unit
- Buses.
---------------------
---------------------
---------------------
Components of microcontrollers:
1- CPU:

It fetches, decodes, and executes instructions.

Components of a CPU:
1. ALU.
2. Instruction Decoder, or control unit.
3. Accumulator.
4. Register file.
5. SFR Registers.

Instruction Decoder:
--------------------
This unit goes to program memory, fetches an instruction, translates it into code that the ALU can understand, generates control signals for that instruction, and repeates this for the next instruction.

It drives the whole instruction cycle.

It understands specific instructions grouped together in an "instruction set".

A register called PC or Program counter points to the instruction after the current one, when it's that instruction's turn, it loads it into PC, then into another register IR "Instruction Register", which is the feed to the Control Unit.

Then the PC points to the next instruction.

2- Accumulator:
---------------
This is a register that stores the last result from the ALU, mainly used for fast calculation of stuff if needed.

3. Register file:
-----------------
A group of registers.

They have n data bits and k address bits (A1A0B1B0), depending on their size and number respectively.

Reading a register:
- Address bits A1A0B1B0
- Read signal WR = 0

Writing into a register:
- Address bits
- Data bits 
- WR = 1

AVR:
----
32 regs.
R0 -> R31

Intel:
------
4 regs only.

4. SFR Registers:
-----------------
These are special registers used for special purposes:
- IR: contains instruction after fetching it from program memory.

ATmega16 has:
16 KB Flash EEPROM for program storage.
AVR Instructions are mostly 16 bits
So flash EEPROM is 8KB*16.

- PC: holds address of next instruction, after fetching it increments PC to step through the program.

The PC in ATmega16 is 13 bits.

- SP: Stack Pointer:
A register that holds an address to the next available location on stack in data memory.

Stack is implemented as growing from higher memory locations to lower ones, so a PUSH decrements SP and a POP increments SP. (That isn't a universal rule for all CPUs)

- Stack: it's used to store temporary data:
1. Local vars.
2. Return addresses after interrupts and function calls.

SP is implemented as two 8-bit regs, low and high.

This depends on SRAM size.

SRAM size in ATmega16 is 1KB so 1K*8-bits, 2^10*8 therefore SP is 10 bits.

SP is init to last memory loc in SRAM.

- Index register:
Holds address when certain addressing modes are used:

Addressing modes:
-----------------
1. Direct: by using address directly.
	e.g. LDS R0, 100

100 is a memory address.
	
2. Indirect: by using index register that holds address.
	e.g. LDD R0, X
	e.g. LDD R1, Y+1

where X, Y, Z are index registers holding certain values in the memory and X+1 is the next place after the address containedf in X.

Usually X is R26 and R27, Y is R28 and R29, and Z is R30 and R31, in AVR.
	
3. Immediate: where we use constant values directly without memory addresses.
	e.g. LDI R0, 15
	
4. Regular: using registers.
	e.g. ADD R1, R2

- Processor Status Register (PSW):
It contains info and result of many ALU ops, used for altering program flow and perform conditional ops.

Bits in PSW:
	- Carry flag: indicates a carry.
	- Zero flag.
	- Negative flag.
	- Overflow flag.
	- Sign flag: indicates the sign.
	- Half-carry flag: indicates carry at first 4 bits.
	- Bit Copy Storage: to store a specific bit.
	- Global Interrupt Enable: to enable interrupts.
	
	
Cycle of a CPU instruction:
- PC points at i.
- IR loads i.
- PC -> i+1
- IR feeds i to control unit.
- Control unit decodes i.
- Control unit generates control signals.
- Control signals sent to ALU.
- Control Signals read regs from reg file.
- Control signals R/W depending on i.
- ALU executes i's control signals after getting operands.
- ALU writes result in distination.
- End.

Pipelining
--------------------
CPU arch methodologies:
---------------------------
1. Von nuuman:
	- Has single memory for data and instructions.
	- Single data bus so simpler hardware.
	- May wait for read/write operation before fetching new i.
	- Simple and economic.

2. Harvard: (used by microcontrollers)
	- Separate memories.
	- Separate buses (CPU to RAM, CPU to ROM).
	- Can execute i and perform memory access at the same time.
	- Faster but more complex hardware.
	
CPU arch num of instructions:
--------------------------------
1. RISC: (used by microcontrollers)
	- Limited no. of i.
	- Simple i.
	- quick execution of i.
	- load/store only operate on memory.
	- Gains speed but code size will be larger.
	- More times of memory access.
	- Complex compilers.
	- Uses many regs to avoid storing vars on stack.

2. CISC:
	- Many i.
	- Complex i.
	- Slow time for each i.
	- i can operate on memory.
	- Limits the program size.
	- Less times of memory access.
	- Easy compilers.
	- Small number of regs.
	
	
** Block Diag of AVR **


Memory:
-------
1. RAM:
	- Random, we can access any memory location directly.
	- Volatile.
	- Stores data as long as microcontroller is powered and running.
	- Variables are allocated in it.
	- Can be modified by program instructions.

	1.1 Dynamic (DRAM):
		- Has paired transistor and cap, the cap holds charge for short period.
		- requires periodic refreshing.
		- refresh cycle has priority over any memory access operation.
		
	1.2 Static (SRAM):
		- many transistors, no caps.
		- use pairs of logic gates to hold bits.
		- no refreshing.
		- density is lower AND cost is higher compared to DRAM.
		
*** SRAM and DRAM block diagrams ***

		DRAM is slower and cheaper.
		So used for large RAMs of PC, workstations, etc.
		
		SRAM is used for cache memory of these computers.
		
	1.3 Non-volatile NVRAM:
		- Has backup battery power to retain content after power shut off.
		
		- Or when power is shut off it writes contents to EEPROM then reads it back when power is back.
		
RAM is divided virtually into:
1. GP regs as accumulators (AVR)
2. Peripheral control regs e.g. i/o ports, UART, timers, etc.
3. Global and static variables are stored on RAM.
4. Stack.

SRAM in ATmega16:
-----------------
- first 32 locations are reg file.
- next 64 locations are i/o, ADC,timers, USART, etc.
- then 1024 locations address internal data of SRAM.

Total: 1120 data bytes.

Memory mapped i/o: using same address bus to address peripherals AND memory.

So addresses used by i/o devices are reserved and not available for normal data.

So it reduces memory available.

2. ROM:
-------
- used as program memory.
- non-volatile.
- slower than RAM.
- Writtin upon programming the controller and can't be modified at runtime.
	
2.1: Masked rom:
	- programmed by manufacturer.
	- very low price.

2.2: OTP (one-time programmable):
	- or PROM.
	- Can be programmed once only.
	- Used when the firmware is stable.
	
2.3: UV EPROM:
	- Erasable understrong UV light.
	- After some time we can program it again.

2.4: EEPROM:
	- Can be erased by electricity.
	- But erases byte by byte which is slow.
	- Often used to read and store vars created during runtime which need to be permanently saved e.g. passwords.
	- Used as peripheral.
	
2.5: Flash EEPROM:
	- Same as EEPROM but erases in Sectors not bytes.
	- So it's much faster.

Flash EEPROM in ATmega16:
-------------------------
16K flash, organized in 8K*16 since AVR instructions are 16/32-bits.

Divided into two sectors: Boot program and App program.

PC is 13-bits.

Finite number of write/erase cycles 10,000.

Most microcontrollers use flash eeprom.

Usually fast to read but slower to write.

What does ROM contain?
----------------------
1. Program code.
2. Interrupt vector table.
3. Constant data "const" keyword.
4. Boot-loader.

IO ports:
---------
MC has one or more registers "ports" that are bidirectional (input or output).

If we want to get input or give output, we connect through some pins that compose these ports.

Each port is controlled by an SFR, each bit determines state of that pin.

So a port is a bunch of pins that are used for i/o, and in software we control them using a register SFR that represents that port with its bits.

Timer:
------
8, 16, 32-bit timers incremented by each clock cycle.

Timers are REALLY useful in embedded MC and each MC has at least one.

They're used for timing events, counting events, time passed between two events, generate interrupts, generate PWM, etc.

Watchdog timer:
---------------
A countdown timer that triggers system reset if reached zero.

When we write time-critical software we start watchdog timer and stop it after the task.

If the timer didn't stop and reached zero, that means something happened in the task between the start/stop of the timer.

So the timer fires reset.

It's like resetting your PC when it's stuck but in embedded apps.

CPU also restarts the timer regularly, if it didn't then it means a hardware fault so the timer fires.

Buses:
------
The wires connecting the components, usually 8, 16, 32 wires, each carries one bit.

Types:
	1. Address bus:
		- Unidirectional.
		- Carries addresses of mem locs.
		- #wires in address bus determines total no. of mem locs.
		
	2. Data bus:
		- bidirectional, carries data.
		- 8-bit microcontroller means the data bus is 8-bits.
		- #wires in data bus determines #bits in each memloc.
		
	3. Control bus:
		- Carries commands from CPU and returns status from devices.
		e.g. R/W and CS (Chip Select).
		
Interrupts:
-----------
Events that require immediate action by MC.

Signal emitted by HW or SW.

MC pauses current task, goes serves the interrupt (ISR), then continues the paused task.

How to choose an MC:
--------------------
Clock: signal that oscillates between high and low.
	- Internal or external
	- External can be RC or crystal.
	- Internal RC only.
	
	Internal:
		- Crystal:
			Quartz Put between two caps connected to gnd.
		- RC:
	
Power: 3-5.5V, we want as less power as possible.

To minimize power:
1. Turn off unused logic.
2. Reduce memory access.
3. Use sleep modes in unused MCs.


			