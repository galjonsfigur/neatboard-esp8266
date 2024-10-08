# 10.10.2021

Some design constraints:
- ESP8266EX
- DN007 PCB antenna -> 1mm PCB needed
- D1 pin compatibility
- USB-UART with autoreset
- built-in 3V3 regulator
- elements only on one side (solder jumpres allowed on the other)

After searching for suitable USB-UART IC i think the best fit will be CP2102 in QFN28.
Other options:
- CY7C6521X - costly and additional programming needed
- CP210X in QFN20 or QFN24 packages - hard to obtain (it's really bad when the
cheapest way to get the IC is to get the brakout board and desolder it)
- FT232 - too expensive, too big
- FT230X - does not have additional signals for autoreset and making additional
lib to use one of GPIOs to toggle reset of the ESP is just too much fuss
- CH340X - unreliable because of the drivers, shame
- PL2303 - too big
- some uC with USB-UART firmware - not really worth the fuss, would be hard to get higher bauds, at least on cheaper ones without external crystal

Things to look for:
- SMD side button for reset
- 3V3 regulator (some little DC-DC converter would be nice)
- maybe 2 mosfets in one package for space saving (for autoreset circuit)
For 3V3 regulator there are two options:
- LDO: ME6211 or 1117 for example
- DC-DC Step-down converter like LM3671
The second one requires bit more space and one more component (inductor)

# 11.10.2021
Settled for LM3671 DC-DC converter - should work just fine and while it requires quite a bit more elements (2 big caps, one big inductor and smol diode to not power CP2012 when external 3V3 supply is used) BUT it has lower Iq and better Imax than LDOs like ME6211.

For auto-reset circuit PUMH11 will be used (cheapest dual NPN "digital" transistor in SOT363 i could find on aliexpress)

Note to myself - do not forget about the diode for dc-dc converter!

Found some of the components needed (2.2uh inductors in 3x3x11mm packages, 10 and 22uF 0805 caps for extra stability (it should be better than 4.7 and 10uF combo from the datasheet - need to test that).

Also not to forget to get some 0402 SMDs - I will try to use only them 

Things to consider/find:
- SMD Fuse
- reset button

# 12.10.2021
For button 1TS003B will be used - small and cheap enough,should fit neatly on edge of the PCB, somewhat like in D1 mini. Need to design footprint for it though.

Also found 0402 0.5A fuse (but that semms almost too small but will try it anyway)

Ordered CP2102 boards and some additional components. For now that should do as
I got the elements for at least one board somewhere. Time to design the schematic.

Another thought - I could leave some room on the bottom to write MAC adress for example

Problems so far:
- CP2102 shouldn't be turned on when the power is supplied from 3V3 pin - that
might make little problem with lack of TXD pullup - might want to add some big
R to compensate that (all according to https://tinker.yeoman.com.au/2016/05/29/running-nodemcu-on-a-battery-esp8266-low-power-consumption-revisited/ )
- PUMH11 has additional base-emitter resistor but it shouldn't be a problem.

Tested the original TI design - easiest way is to import the gerber file and then put in into pcbnew
there is a slightly difference from the original design. The feed line position differs - I will try to fix it rather than importing TI's design

To do for now:
- footprint for the button
- symbol for the antenna + check if the footrpint is correct (maybe just import TI's dxf->swg->kicad?)
- desolder ESPs and sort elements from them
- search parts bin for 0402 parts
- add all the resisotrs for the esp
- fix the antenna footrpint

# 13.10.2021
Fixed antenna footprint - it should be fine now. 
I realized that board thickness won't really matter - there is no ground plane
under it, so the only crucial thing to calculate would be waveguide line width.
I don't like the fact that with 2-layer PCB it's hard to make a thin transmission
line.

Rough schematic is mostly ready - a few touches here and there and maybe add
TVS diode in SOT-143 for CP2102 - great thing about it is that it can
even stay unpopulated so if I couldn't get any I don;t have to do ugly solder bridges.
Things still left to do:
- desolder ESPs and sort elements from them
- search parts bin for 0402 parts
- find TVS diode array in SOT-143 or simmilar package
- adjust R according to wat I have in parts bin
- draw outline of the PCB (another thing about the IFA antenna is that I still
can round corners and it shouldn't really affect the performance that much - even if,
I don't have VNA or SA or anything to check S parameters or smith chart or anything really
- recheck the schematic with attention to pin numbers

# 14.10.2021
Day off

# 15.10.2021
Added TVS diodes and searched parts bin. Found very nice 0402 Vishay 1% resistors and 100nF 0402 caps - neat.
Grabbed USB footprint from https://github.com/choryuidentify/KiCAD-USB-Micro-B-Chinese-SMT and slapped Molex step file to it - good enough for me.
Assigned rest of the footprint and imported schematic to PCB to test 3D view - looks good.
Still need to check schematic for potential mistakes though.
Also when searching around I found Dialog appnote with some very pretty antennas - when I get some VNA I will probably make PCB with
SMAs to check all those designs out of curiosity

Made very rough PCB - it seems like it's gonna fit just fine - we will see

#16.10.2021
Finished PCB outline and locked important components in place (pin headers, antenna, USB, reset switch)
Tested various routes - SPI-ESP can be done with just 2 vias, USB with ESD protction has to be done
with via bridge or 0R resistor (D+ and D- on QNF package does not correspond with micro USB socket - why Sillabs, why)
PI network is kinda the issue now - maybe when I move ESP a little down it would fit nicely with one straight path
Also KiCad default track width (0.25mm) corresponds VERY nicely with QFN packages, but new classes have to be added for Power and LNA. 

#17.10.2021
First full route of the whole thing - looks good enough but requires many tweaks

#18.10.2021
Day off

#19.10.2021
Day off

#20.10.2021
Day off

#21.10.2021
Day off

#22.10.2021
Done mostly nothing, will have to set all the silkscreen stuff and wonky traces.

#23.10.2021
Done First rev of the board, have to add some descritption and put the project on the shelf until I will have money to get those PCBs.




