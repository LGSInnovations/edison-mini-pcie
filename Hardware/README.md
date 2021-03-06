mPCIe Edison Block Hardware Guide
=================================

The mPCIe has a downstream USB hub connected to the Edison's OTG port. This allows an additional downstream USB for the user, and one for the mini PCI-express connector. This block also has a SIM card spot, circuitry to detect battery usage on the Edison, and a USB-to-UART port for Linux debugging.

This board is intended to be a termination block, i.e., there is no bottom Hirose connector.

## Components ##

| Component 		| Description										| Resources                                                                                                  |
| :-----------------|:--------------------------------------------------| -----------------------------------------------------------------------------------------------------------|
| USB2512b 			| USB 2.0 High-speed hub w/ 2 downstream ports 		| [Datasheet](http://ww1.microchip.com/downloads/en/DeviceDoc/00001692.pdf)
| FT231XS-U 		| USB-to-UART FTDI chip								| [Datasheet](http://www.ftdichip.com/Support/Documents/DataSheets/ICs/DS_FT231X.pdf)
| TPS62065 			| 2A 3MHz DC-DC step-down converter, set to 4v		| [Datasheet](http://www.ti.com/lit/ds/symlink/tps62065.pdf)
| BA33DD0WHFP 		| 3.3v/2A LDO 										| [Datasheet](http://rohmfs.rohm.com/en/products/databook/datasheet/ic/power/linear_regulator/baxxdd0-e.pdf)
| BA15BC0FP 		| 1.5v/1A LDO 										| [Datasheet](http://www.mouser.com/ds/2/348/baxxbc0fp-e-209385.pdf)
| MAX4995AAUT 		| USB Port Power current limiter IC, set to ~480mA  | [Datasheet](http://datasheets.maximintegrated.com/en/ds/MAX4995A-MAX4995C.pdf)
| BSS138 			| MOSFET to control `DCIN` input to Edison			| [Datasheet](https://www.fairchildsemi.com/datasheets/BS/BSS138.pdf)

## Power Requirements ##

To power the mPCIe block and the Edison, at least a 5v/2A connection is required to the USB-to-UART console port. This 2A is used by the following major components:

| Component                | Current Draw                    |
| :----------------------- |:--------------------------------|
| The Intel Edison         | ~250mA (3.3-4.5v @ <1W)         |
| The USB Hub (USB2512b)   | 155mA					         |
| mini PCI-express device  | typically 500-700mA w/ 2A spikes|
| downstream usb device    | 480mA maximum (due to MAX4995)  |

#### Powering the Block ####

Power comes into the block through the USB MicroB (JP1) console port. This voltage is then stepped down using a 5v to 4v/2A 3MHz converter (TPS62065). This feeds `VSYS`, which powers the Edison. The Edison converts this `VSYS` to 3.3v on pins 8 and 10; each can source 100mA, giving a theoritical 200mA given that these pins are tied together. Edison also converts to 1.8v on pin 12, also sourcing 100mA.

`VSYS` also feeds a 3.3v/2A LDO and an optional 1.5v/1A LDO. These LDOs power the mini PCI-express card.

The downstream USB is powered directly by the USB-to-UART's `USB_VBUS` pin. It is current limited by the MAX4995AAUT IC to ~480mA.

Consider using a [y-cable](http://www.amazon.com/StarTech-3-Feet-Cable-External-Drive/dp/B0047AALS0) to provide power and the console when using a computer.

#### Current Spikes ####

Because many devices that would be insterted into the mini PCI-express slot can experience up to 2A transient current spikes, a tantalum 470uF capacitor is placed near the 3.3v power supply.

---------------------------------------

## Resources ##

| Item                     | Description                     | Resource
| :----------------------- |:--------------------------------|:--------------------------
| SparkFun Base Block      | Power and FTDI example          | [Schematic](https://cdn.sparkfun.com/datasheets/Dev/Edison/Base_Block.pdf)
| USBMA Wireless Adapter   | mPCIe, power, SIM, example		 | [Schematic](http://www.hwtools.net/PDF/USBMA_ver1.2f_schematic.pdf)
| USB2514b Dev Board       | Example of USB2512b IC          | [Schematic](http://ww1.microchip.com/downloads/en/DeviceDoc/evb2514qfn48.pdf)
| USB2514b Dev Board       | Parts List                      | [BOM](http://ww1.microchip.com/downloads/en/DeviceDoc/evb2514qfn48bom.pdf)
