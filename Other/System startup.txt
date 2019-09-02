What happens when you start the computer?
------------------------------------------
A) POST (Power-On Self Test)
The POST checks the BIOS, then tests the CMOS RAM, then the CPU, hardware devices (e.g. video card), storage devices e.g. hard drive, floppy drivers, zip drive, CD/DVD drive.

If there are any errors a number of beeps are heared known as POST beep codes.

Then, the BIOS starts searching for the MBR on the hard drive, which is usually located at the first location of the drive.

B) MBR (Master Boot Record)
A small program that starts when the computer is booting, in it, stored the bootstrap loader.

When the POST finishes the BIOS starts the BootLoader which loads the OS of the computer into RAM, then the computer will be able to access, load, and run the OS.

C) init
The computer looks for the file /etc/inittab to see if there's an entry for initdefault which determines initial run-level of the system.

A run-level decides the initial state of the OS:
	0 -> System halt.
	1 -> Single user mode.
	etc.
	
D) init 2
Start various daemons that support services e.g. X server daemon supports display, computer, mouse, etc. i.e. when X server daemon is started you see a GUI and a login screen in front of you.
	
This sequence of steps is knows as System Five.

There are others like:
- Systemd
- Upstart