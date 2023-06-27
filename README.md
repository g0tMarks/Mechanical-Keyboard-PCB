# Mechanical-Keyboard-PCB
 This is a writeup of my first mechanical keyboard PCB build where I will be following Masterzen's tutorial so in reality, most of this is a copy of his blogs with edits as necessary, so all credit to him and please check his out:

 - [Masterzen](https://www.masterzen.fr/2020/05/03/designing-a-keyboard-part-1/)

 I also referenced these great resources:

 - [ai03](https://wiki.ai03.com/books/pcb-design)
 - [Ruiqi Mao's PCB guide](https://github.com/ruiqimao/keyboard-pcb-guide)

The most important part of this is whole process is the initial setup stage, where you learn to setup your workspace, organise your directories, correctly install your **local** libraries and then store everything in a Github repository. Let me emphasis the importance of installing all libraries as local libraries - trust us on this one - read [here](https://wiki.ai03.com/books/pcb-design/page/pcb-guide-part-2---beginning-the-project) for more information on why.

## Table of contents

1. Setting up
2. Beginning the project
3. Creating the Schematic

## Setting up


## Beginning the project
Once you have everything set up in GitHub, you need to go to [keyboard-layout-editor](http://www.keyboard-layout-editor.com/) to create your layout.

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/4d17aea1-72b0-4b11-980b-068c3d02a138)


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

h - Place a label

And a lot more shortcuts [here](https://shortcutworld.com/KiCAD/win/KiCAD_Shortcuts)

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

Let’s do the same with the D+/D-.

The next pin to wire is HWB. HWB is forced to GND with a pull down to make sure the MCU will boot with the boot-loader (refer to the data-sheet for more details). Create a R_small symbol for the resistor (we’ll use R_small symbols for all other resistors), then wire it like this:

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/a28775dc-76a3-4bcf-9ecc-65a5b3e63050)

The UCAP is the internal USB pins voltage regulator, it has to be connected to a 1µF capacitor as instructed by the Atmega32U4 data-sheet. Use a C_small symbol for the capacitor (and all capacitors going forward)

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/0d283767-8bde-47ea-90a2-3f6e574d688e)

AREF doesn’t need to be wired, we’re going to mark it with a cross by pressing q and clicking on the pin. AREF (and AVCC FWIW) is used when doing analog signaling which we’re not going to do in our keyboard.

### Hooking up the clock

The next step is to design the clock that drives the MCU and that will be hooked up to the XTAL1 and XTAL2 labels.

The [Atmega AN2519 tech-note](http://ww1.microchip.com/downloads/en/Appnotes/AN2519-AVR-Microcontroller-Hardware-Design-Considerations-00002519B.pdf) gives a recommended design and equations to compute the capacitance values. 

Place a Crystal_GND24_small on the grid close to the MCU. Then wire it like this, keeping the shunt capacitors ground symbol separate from the XTAL ground symbol:

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/a6c31641-aba8-46ff-917d-25dfdae7c794)


In Kicad, every component has two important properties: the reference and the value. The reference is typically a combination of letters and numbers that uniquely identifies a component on the schematic. On the other hand, the value can be anything and serves different purposes depending on the component type. For passive components like resistors and capacitors, the value represents their respective units (ohms for resistors, farads for capacitance, etc.), while for ICs (integrated circuits), it usually denotes the component name.

When a new component is added to the schematic in Kicad, the reference is initially set as one or more question marks (?). This indicates that the reference hasn't been assigned yet. To automatically assign references to all the components, Kicad provides an operation that can be used. This step is necessary for tasks like creating a PCB (Printed Circuit Board) or running the Electric Design Rule Checker, among others.

To edit the values and references of a component in Kicad, you can hover the mouse pointer over the symbol and press the 'e' key. This will allow you to modify various aspects of the component, including its reference, value, symbol, and footprint. Additionally, there are shortcuts available to directly edit the value ('v') or the reference ('u').

If you need to reposition the reference or value label of a component to prevent them from overlapping with wires or other elements, you can press the 'm' key while the mouse is positioned over the component. This action enables you to move the labels to more suitable locations within the schematic, ensuring clarity and readability.

### Power decoupling

The datasheet for the Atmega32U4 microcontroller recommends the use of decoupling capacitors on each +5V pin of the MCU. Decoupling capacitors play a crucial role in maintaining the stability of an active IC. When the component starts drawing current during its operation, it can cause a drop in the voltage of the power source. This voltage drop can be problematic not only for the component itself but also for other components connected to the same power source, as it introduces noise on the power line.

To mitigate this issue, decoupling capacitors are added to each power pin of the IC. These capacitors act as local energy storage. When the IC begins consuming energy, the capacitors provide the necessary current without significantly affecting the power source. When the component is not actively drawing power, the decoupling capacitors gradually recharge, preparing for the next power demand.

According to the [AN2519 technical notes](http://ww1.microchip.com/downloads/en/Appnotes/AN2519-AVR-Microcontroller-Hardware-Design-Considerations-00002519B.pdf), every VCC pin of the MCU should be decoupled using a 100nF (or 0.1µF) capacitor.

For optimal effectiveness, the decoupling capacitors should be placed as close as possible to the MCU on the final PCB layout. In the case of the Atmega32U4, there are four VCC pins (2 AVCC, UVCC, and 2 x VCC), so ideally, five 100nF capacitors are needed, along with one 10μF capacitor for VBUS. In practice, it is possible to share the 10μF capacitor for both VBUS and UVCC and distribute the four 100nF capacitors among the remaining VCC pins.

To avoid cluttering the electronic schematic, as suggested by ai03, the decoupling capacitors are grouped together in the schematic diagram.

To implement this in the schematic, start by placing one capacitor and then use the 'c' command to copy and move the subsequent capacitors until all of them are placed. Finally, wire them according to the provided schema to establish the necessary connections.

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/74ccb0f1-7be8-4ca6-8450-f07749a85c8b)

### ISP header

In the event of a catastrophic failure or the loss of the DFU (Device Firmware Upgrade) bootloader, it may become necessary to reprogram the Atmega32U4 microcontroller. In such cases, the USB port cannot be used for programming purposes. Instead, the Serial Peripheral Interface (SPI) programming interface and [ISP (In-System Programming)](https://docs.qmk.fm/#/isp_flashing_guide) mode need to be accessed.

To facilitate this, a 6-pin header with the necessary SPI signals will be included on the PCB. This header will provide a convenient connection point for the ISP programming mode. The specific signals required for the SPI programming interface will vary depending on the specific implementation and requirements. However, commonly used signals include:

1. MISO (Master-In Slave-Out): This signal represents the data line from the slave device to the master device in SPI communication.

2. MOSI (Master-Out Slave-In): This signal carries data from the master device to the slave device during SPI communication.

3. SCK (Serial Clock): This signal is the clock line that synchronizes data transfer between the master and slave devices.

4. SS (Slave Select): This signal is used to select the specific slave device for communication when multiple slave devices are connected to the same SPI bus.

5. RESET: This signal is responsible for resetting the microcontroller during the programming process.

6. VCC (Power Supply): This pin provides power to the programming interface.

By including this 6-pin header on the PCB, it becomes possible to connect an ISP programmer or another compatible device to the Atmega32U4 microcontroller for reprogramming purposes. 

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/b8f9dda8-c52d-48d4-a1f0-92bd9ed61b9f)

And associate it with the corresponding pins on the MCU:

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/933fd471-0111-42f9-bcf9-332bf2b3e8ef)

It's important to note that the three signals required for the SPI programming interface consume three general I/O pins on the microcontroller, which could potentially be used for connecting the matrix. In the case of a matrix like mine with 19 rows and 6 columns, it would require a total of 25 I/O pins on the MCU. Therefore, I will need to share the ISP pins with the matrix. This approach takes advantage of the fact that during ISP programming, the matrix lines won't be in use, and during regular keyboard use, the ISP pins won't be in use.

There are alternative matrix configurations that can be implemented to overcome the constraints of a limited number of pins. See [this](https://wiki.ai03.com/books/pcb-design/page/matrices-and-duplex-matrix) page for further reading on matrices and the duplex matrix.

### Reset Button

Let's hook up a reset switch. For this, you'll want a switch (SW_PUSH) named SW1 and a 10k resistor for pullup (R) named R1. If you want to know why we want a pullup resistor and what a pullup resistor even means, [here](https://learn.sparkfun.com/tutorials/pull-up-resistors) is a good explanation from Sparkfun. But for now, here's how it should be hooked up:

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/ac7607fe-b4f1-47d9-a076-2a1ed207dfe8)

### USBC 2.0

### ESD

### Matrix

![image](https://github.com/g0tMarks/Mechanical-Keyboard-PCB/assets/37822503/cc581e17-3612-45f0-955f-608f4f0cf5cb)
