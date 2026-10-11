Hi-speed papertape reader and punch emulator for Data General Nova

This Arduino-based unit emulates the "high-speed" papertape reader and punch for Nova computers. It connects to the 4011 and 4013 controllers on the Basic I/O card in the Nova.

--------------

Parts list:

PCB (gerber file)

Front panel (gerber file)

3D-printed bracket for microSD module (STL file)

3D-printed "201_RK05em_Bezel_v06.stl" from G-Wiley RK05 emulator on GitHub

Arduino Mega 2560 Rev 3. Mouser 782-A000067

Adafruit I2C Stemma QT Rotary Encoder Breakout with Encoder (product code 5880). Mouser 485-5880

Eagle knob. Mouser 450-4667

Adafruit MicroSD card breakout board+ (product code 254). Mouser 485-254

A standard microSD card

Adafruit Assembled 0.56" 4-Digit 7-Segment Display - w/ I2C Backpack STEMMA QT - Red (available in other colors). Mouser 485-5599

Adafruit STEMMA QT / Qwiic JST SH 4-pin to Premium Male Headers Cable - 150mm (code 4209). Mouser 485-4209

Adafruit STEMMA QT / Qwiic JST SH 4-pin Cable - 100mm (code 4210). Mouser 485-4210

2 x simple toggle switches, SPDT, 6 mm hole.

2 x PCB header strips. Mouser 200-HTSW14007LS

4 x 20-pin IDC female connectors. Mouser 571-1658623-4

2 x 20-pin IDC through-hole connector male, angled. Mouser 571-2-5499345-4

4 x Mounting fixings, M3 brass. Mouser 153-IUB-M3-2

4 x 30 mm standoff, M3. Mouser 144-CBMFTS335A

10 x Nylon hex nuts M3. Mouser 534-4688

10 x Nylon screws, M3 x 15 mm. Mouser 144-50M030050H015B

20-way ribbon cable, 2 m

Optional: HCPL-4100-000E Broadcom logic output optocoupler. Mouser 630-HCPL-4100-000E

Optional: 1 nF ceramic capacitor

--------------------

Vague building instructions:

Solder header strips to PCB on the TOP side (with text like SDA, SCL, 5V, GND, L/R etc).

Solder male IDC connectors on the BOTTOM side (with the Da-Da logo etc.).

The Qwiic 4-pin to male headers cable is soldered into the SDA, SCL, 5V and GND pins on the PCB (I2C interface), mind the correct color codes, and connect to the LED display.

Continuing from the other connector on the LED display, the 4-pin to 4-pin cable is daisy-chained to the rotary encoder.

Short wires are soldered from the microSD breakout board to the SS, COPI, CIPO, SCK, GND and 5V pads on the PCB.

Screw the microSD breakout board onto the microSD bracket, and the bracket onto the PCB.

Mount rotary encoder and 2 switches on the front panel.

Connect the two center pins of the switches with a short wire, and further to GND on the PCB.

Connect the upper pin on the Read/Punch switch to R/P on the PCB, and the upper pin on the Load/Run switch to L/R.

Program the Arduino with the provided source code, and insert into the header strips on the PCB.

Glue the LED display to the front panel.

Push/melt the mounting fixings into the bezel with soldering iron.

Glue the front panel to the bezel.

Fasten PCB to bezel with the standoffs and screws

---------------------

Nova wiring:

Make two ribbon cables, e.g. 1 m long, with 20-pin female IDC connectors at one end. These connectors plug into the emulator. Note where pin 1 is (as marked on the PCB).

At the NOVA end, there is now some very tedious wiring to do. I used 15-pin, 3-row male D-sub connectors that plug into female D-subs on the NOVA, and then via ribbon cables to 20-pin female IDC connectors that go into male headers at the bottom of the NOVA backplane.
I can make detailed wiring instructions if anyone is interested.

----------------------

20 ma current loop issue:

The SYNC signal from the emulator to the 4013 controller is 20 mA current loop in the original 4013. If you have that, then you must mount the optocoupler and capacitor on the BACK side of the PCB. My Nova did not have installed the current loop circuitry on the Basic I/O card, and so I just connected directly as TTL. If you have the same, you don't need the optocoupler and capacitor, but must place a bypasss jumper from pin 3 to pin 6 where the optocoupler is not.

-------------------

How to use:

Supply power to the Arduino with some power supply.

"Paper tapes" on the microSD card can be provided from a computer with a microSD slot, or via the punch function. They must be named 0.ptp, 1.ptp etc. up to 99.ptp. You must keep a record of what these are (I maintain a text file on the microSD for this).

Read a tape: Make sure the Read/Punch switch is in the Read position and the Load/Run switch is in the Load position. Select tape with the rotary switch. Switch to Run, and initiate the read from the Nova. The display will show bytes transferred. When finished, switch back to Load.

Punch a tape: Make sure the Read/Punch switch is in the Punch position and the Load/Run switch is in the Load position. Select tape number with the rotary switch. Switch to Run, and initiate the punch from the Nova. The display will show bytes transferred. When finished, switch back to Load.

----------------

There is NO WARNING if you overwrite an existing tape.

In Extended BASIC, you can punch the program in binary with the command SAVE "$PTR". Load with LOAD "$PTR".

The punch is intentionally slow to mimic the old punch. In a future version I might add a "turbo" switch.
