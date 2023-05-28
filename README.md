# Mechanical-Keyboard-PCB
 This is a tutorial of my first mechanical keyboard PCB where I will be following 3 tutorials to build my own custom PCB:
 
 - [Ruiqi Mao's PCB guide](https://wiki.ai03.com/link/24#bkmrk-from-ruiqi-mao%27s-pcb)
 - [ai03](https://wiki.ai03.com/books/pcb-design)
 - [Masterzen](https://www.masterzen.fr/2020/05/03/designing-a-keyboard-part-1/)

The most important part of this is whole process is the initial setup stage, where you learn to setup your workspace, organise your directories, correctly install your **local** libraries and then store everything in a Github repository. Let me emphasis the importance of installing all libraries as local libraries - trust us on this one - read [here](https://wiki.ai03.com/books/pcb-design/page/pcb-guide-part-2---beginning-the-project) for more information on why.

## Table of contents

1. Setting up
2. Beginning the project

3.[ Creating the Schematic](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/edit/main/README.md#creating-the-schematic)

## Setting up


## Beginning the project
Once you have everything set up in GitHub, you need to go to [keyboard-layout-editor](http://www.keyboard-layout-editor.com/) to create your layout.

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/fd757c3f-b3c4-4f8c-ab19-0edb2a49b1ad)

Once you have a layout, you need to figure out your rows and columns. 


## Creating the Schematic

Open the schemait editor by selecting the '.kicad_sch'

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/32c0f618-3128-439a-94da-480f43c86f91)

Once in the schematic editor, press 'a' to open up the symbol library and insert the atmega32u4 MCU. I will use this specific MCU primarily because it matches the package/case (TQFP-44) of the -au version I will buy from au.mouser.com that is a hand-solderable, unlike the MU variant which is a QFN package - the pins are below the case making it harder to solder with a standard soldering iron.

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/b93087e0-8d9c-4700-a14e-ed060c084943)

Insert the MCU into the schematics and place it somewhere close to the right of the page:

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/4ff5afa8-df17-496c-afbc-ee83b97c5c60)

These are some helpful shortcuts from [Ruiqui Mao's PCB guide:](https://github.com/ruiqimao/keyboard-pcb-guide)

m: pick the component up and move it
g: drag the component up and move it while keeping wires attached to it
c: copy the component
e: edit the component
r: rotate the component
y: mirror the component
del: delete the component
esc: abort!
w - Begin drawing a wire connection
k - Cut a wire and stop drawing it without clicking on an endpoint
Ctrl + h - Place a global net

Then let's tie UVCC, VCC, and AVCC to +5V, as specified by the datasheet.
Press P to open the power symbols menu, and select the +5V symbol.

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/2f4cd521-634e-4977-b98b-6207bf000a1e)

Place it near the top of the MCU and wire connect the UVCC, VCC and AVCC wires to it.
Press w to place a wire.

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/28e567c3-25d2-4b3f-96b9-459413112d77)

Next, GND and UGND should be connected to the GND power symbol.
Press p to open the power symbols menu again and select the GND symbol. **Do not use the GND from keyboard_parts library, it does not register a ground layer in the PCB editor**

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/b224ecf3-580b-4e1d-a469-4b803532a8d8)

**Protip**: It is good practice to have positive power source symbols pointing up, and ground power symbols pointing down.

The MCU needs a clock to be able to properly sequence instruction execution and make sure that the timing and response of each key stroke is accurate. We will be using a crystal oscilator that produces a 16MHz signal. This will be connected to XTAL1 and XTAL2. We can place labels on those pins and then position the crystal wherever you like.
Press h to open up the Hierarchical Label Properties and give it the name XTAL1 and XTAL2.

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/81e54be2-9a39-4ce8-bf78-220c3ab9e24a)

**Protip**: Before you place a symbol in your schematic, you can press r on your keyboard to rotate it.

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/c7ff9430-a117-468f-b437-9648ee772f48)

Let’s do the same with the D+/D- and RESET pins.

The next pin to wire is HWB. HWB is forced to GND with a pull down to make sure the MCU will boot with the boot-loader (refer to the data-sheet for more details). Create a R_small symbol for the resistor (we’ll use R_small symbols for all other resistors), then wire it like this:

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/a28775dc-76a3-4bcf-9ecc-65a5b3e63050)

The UCAP is the internal USB pins voltage regulator, it has to be connected to a 1µF capacitor as instructed by the Atmega32U4 data-sheet. Use a C_small symbol for the capacitor (and all capacitors going forward)

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/0d283767-8bde-47ea-90a2-3f6e574d688e)



