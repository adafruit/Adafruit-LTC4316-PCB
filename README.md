## Adafruit LTC4316 I2C Address Translator - Stemma QT / Qwiic PCB

<a href="http://www.adafruit.com/products/5914"><img src="assets/5914.jpg?raw=true" width="500px"><br/>
Click here to purchase one from the Adafruit shop</a>

PCB files for the Adafruit LTC4316 I2C Address Translator - Stemma QT / Qwiic. 

Format is EagleCAD schematic and board layout
* https://www.adafruit.com/product/5914

### Description

Adafruit has hundreds of designs that use I2C - a two-wire protocol that can let you quickly connect sensors, OLEDs, GPIO expanders, and more. Folks love I2C because you can simply connect 4 wires for power and data, and even better, share those wires with multiple devices!

One things folks don't like about I2C is that each device requires it's own unique 'address'. You can only have one device with a 0x20 address, for example, so while in theory you could have up to 127 device, each for the 7-bits of address space available, in reality you will quickly bump into two sensors with the same address. Many I2C components have jumpers or switches that allow changing the address, but not all. And some only let you pick between two values. One solution is to use an I2C multiplexer,  and these work pretty well, but they require some noodling in your code to set up the 'plexer and then switch between the devices being plexed.

The Adafruit LTC4316 I2C Address Translator is another solution, with some magic sprinkled inside. This chip does on the fly address translation. There's an 'input I2C' half, and an 'output I2C' half. And any devices on the 'output' half will automatically have their addresses translated from the input half. Specifically, each device will have bit A6 flipped  (most-significant-bit of the address) and then bits A4 and A5 can also be flipped or kept the same, with the two DIP switches on board. To determine the translated address, we use XOR bitwise math.

OK that's a little confusing so let's work through an example. Say we have an AHT20 connected on the output side of this breakout. The AHT20 does not have an adjustable I2C address: it's fixed at 0x38. If both DIP switches are ON, then only A6 is flipped. That means the I2C controller on the input side will see 0x38 XOR 0x40 = 0x78 address. If we flip the A5 switch off, now it will be 0x38 XOR 0x60 = 0x58 and if both switches are off, now bits A4, A5 and A6 will be flipped, so 0x38 XOR 0x70 = 0x48. As far as the AHT20 is concerned, it will happily still see I2C writes and reads on 0x38, but from the I2C controller's perspective, the device is responding on the new address.

If you need more than 4 address translation options (two switches give you 4 options) we also have a spot where you can solder an XOR_LOW resistor for setting the 'bottom' 3 bits by soldering in a resistor value according to the datasheet. It can get confusing quickly so we definitely recommend using an I2C scanner to debug. Note also that you have to reset the LTC if you change the translation address with resistors or the DIP switches: the translation value is picked up on chip boot and isn't on-the-fly adjustable without a toggle of the Enable pin.

While this chip is magical, there's a few things to watch for: it doesn't seem to support clock-stretching so not for funky chips like BNO055. Just because you can change the address on the fly, doesn't mean the drive supports it! Some firmware is expecting a specific address and it may not be trivial to change the address. Check the driver to make sure you know how to change the address, to the new value.

To get you going fast, we spun up a custom-made PCB in the STEMMA QT form factor, making it easy to interface. The STEMMA QT connectors on either side are compatible with the SparkFun Qwiic I2C connectors. This allows you to make solderless connections between your development board and the LTC4316 or to chain it with a wide range of other sensors and accessories using a compatible cable. QT Cable is not included, but we have a variety in the shop. 

### License

Adafruit invests time and resources providing this open source design, please support Adafruit and open-source hardware by purchasing products from [Adafruit](https://www.adafruit.com)!

Designed by Limor Fried/Ladyada for Adafruit Industries.

Creative Commons Attribution/Share-Alike, all text above must be included in any redistribution. 
See license.txt for additional details.
