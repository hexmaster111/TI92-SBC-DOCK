
**NOTE**
The Zero version is not currently being worked on and is an unusable state

# TI92 - CM4

TODO
--------
- [ ] Whats gonna happon is im gonna get a 4 inch hdmi display and desolder the hdmi connector and add it to a ribbion cable and then add a connector to the pcb for hdmi to interface with that
- [ ] Add a SD card slot
- [x] Figure out what display to use
- [ ] Add a headphone port
- [x] Add a USB Hub
	- [ ] Route USB in a good way :wink:
- [x] Get a charge solution sorted
- [x] Figure out how the keyboard will work and talk to the pi
- [ ] TEST
- [ ] Write code for the ATTINY85 (aka power control chip)
	- [ ] ~~Find a better way to switch power?~~
 - [ ] Use the micro USB that is used for charging to also act as USB OTG for flashing the CM4
 - [ ] Figure out a low voltage cut off for the batCell+ line
 - [ ] Calculate R4 on the charge chip
 - [ ] Make sure all the components will fit in the case with there given footprints
	 - [ ] Measure for LEDS
		 - [ ] Drill Holes for indicators
		 - [ ] Make a nice overlay for indicators 
	 - [ ] Measure for USB Connections
		 - [ ] Modify case


Display
----------
The display being used is a 4 inch SPI display by waveshare and is connected with a pin header, I still have to get the part and check for fitmint for sure, but [this](https://www.waveshare.com/4inch-RPi-LCD-C.htm) looks prmissing

Battery Charging
----------
For charging two 18650s I'm using the BQ25887RGET Chip by TI. This chip should have enough current to keep the batteries charged in use. For power input I'm using a Micro USB 

USB Hub
-----------
For a USB hub I'm using the TUSB4041l by Texas instruments, this chip supports 4 downstream ports and 1 upstream, the upstream port is wired directly into the CM4's USB data lines. There are two ports that are connected to standard USB connectors. 

There is also one USB port left unused on the board for internal modifications later on. 

To Get this chip to work, I had to add a 3.3v power supply capable of suppling more current then the pi could, and added the +3.3 High Current Global net. 

To Get this chip to work, I had to add a 1v power supply capable of suppling more current then the pi could, and added the +1 Global net.

Making the keyboard work
----------------------------------
For a keyboard controller I chose to use a 5K5126 which is a programable keyboard controller that supports arbitrary key mapping along with macros and this is all done with a graphical program and uploaded to the keyboard. 
After uploading the matrix and stuff to the controller the idea is to then solder JP2 & JP3. This will then allow the keyboard to communicate with the hub controller and then into the CM4 modal
**note:**  The keyboard controller has some GPIO we can use for a backlight or indicator LEDs, I need to check this out more. 


Power Control
--------
The +5v will go high after the ON button on the keyboard is pressed, this is done using an ATTINY85 and a latching relay, this is done so that I can use this for other single board computers in the future.  
Here is some pseudo code to help understand the idea
```
Use a pin on the pi as a way to know if the system is on and if that pin goes low we disconnect the battery from the 5v converter

if power=off & PowerButton=on //Turn on +5v rail (ColdBoot)
	SETPIN goes high
	wait(50ms)
	SETPIN goes low
	wait(1min)

if PiOnPin=low & power=on  //Shutdown pi after power on pin goes low
	RESETPIN high
	Wait(50ms)
	RESETPIN low

if PiOnPin=high & power=on & PowerButton=on //Force Shutdown 
	if PowerButton = held for 10 sec
		RESETPIN high
		Wait(50ms)
		RESETPIN low	
```


Things to Add Perhaps
---------------------------
- A pointing device
	- A track ball or other pointing device like [this](https://shop.pimoroni.com/products/trackball-breakout). 
	- Perhaps a trackpad
	- Use the Lock/Pinch key as a the key for mouse keys and use the D-Pad as the key mouse
- Some way to have a nice GPIO pin header behind a door for a clean look
- A new keyboard overlay for 2nd keys and the green diamond keys
- Custom keycaps?! (Clear injections would be cool for backlighting)
- Perhaps a ti-92 plus emulator for it (just for fun)

Credits
------
I got the PCB and keyboard layout and matrix diagram from [here](https://github.com/ccadic/TI92-revive). 
Everything else was from the Digi key parts repo or I made. 
