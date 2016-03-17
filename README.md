#**Physical Web Toy**
##**Bluetooth Smart Controlled Car**

This project is a bachelor thesis done by four students of electrical engineering at [The Norwegian University of Science of Technology] (http://www.ntnu.edu/).
The project utilizes the [nRF52 Development Kit] (http://no.mouser.com/new/nordicsemiconductor/nordic-nrf52-dk/) (DK for short) from [Nordic Semiconductor] (http://www.nordicsemi.com/), which is a versatile single-board development kit for Bluetooth® Smart, ANT, and 2.4GHz proprietary applications. It is hardware-compatible with the Arduino Uno Revision 3 standard, so it  enables the use of 3rd-party shields that conform to this standard.

The thesis itself is scheduled to be delivered may 25th 2016, so the publication of this project is a snapshot of the functionality to coincide with the release of [The Physical Web] (https://google.github.io/physical-web/). Although the project is not fully realized at the time of publication, most of the functonality is in place, which we'll explain further down in the text.

Our goal was to make several Bluetooth Smart controlled cars that can play a game of laser tag against each other as well as receive random power ups by driving over RFID tags. It is the latter functionality which is not fully realized as we had not recieved RFID trancievers at the time of publication, although the power up function itself is realized in both firmware and software, we have to use an alternative way to trigger it.

The cars are built using a prefabricated kit designed for Arduino and other micro controllers. We have also planned to make a body that goes over the cars using vacuum forming, and 3D-printing parts to attach the bits and bobs to the cars.

Below we will go through the different parts - hardware, firmware and software - in further detail.

##Hardware
###4WD "Robot" Car
The car chassis is a [prefabricated kit] (http://www.banggood.com/4WD-Smart-Robot-Car-Chassis-Kits-With-Strong-Magneto-Speed-Encoder-p-917007.html) consisting of two identical acrylic sheets stacked on top of each other, four DC-motors - one for each wheel - placed between the acrylic sheets and four wheels. The cars were chosen primarily because they were a good platform to experiment and prototype with, but also because we felt it very important to have a small as possible turning radius. The latter we achieved by driving the cars like a tank, where the wheels turn in parallell, making the car capable of spinning around itself.
###Motorshield
To be able to control the voltage that goes to the DC-motors in a good way we decided to get a motorshield that stacks on top of the development kit. The motorshield we use is produced by [Adafruit] (https://www.adafruit.com/products/1438), and is capable of driving 2 hobby servos and 4 DC motors or 2 stepper motors. Since the shield is produced for Arduino and the firmware to drive it doesn't exist for the nRF52 DK, we had to use a [logic analyzer] (https://www.saleae.com/) to decipher the data being sent from the Arduino code to the motorshield, and reconstruct it in the firmware for our DK, to produce our own [I²C/TWI] (https://en.wikipedia.org/wiki/I%C2%B2C) driver. So now other people wanting to use the Adafruit motorshield with the nRF52 development kit can use our code.
###NFC/RFID Controller Shield
To be able to read RFID-tags and realize our power up funtionality we decided to use [Adafruits NFC/RFID Controller Shield] (https://www.adafruit.com/products/789). We chose this because the shield uses the I²C communication protocol as a default, is compatible with Arduino (thus compatible with our DK) and Adafruit has very good documentation of their products.
###Infrared transmitters and recievers
The laser tag functionality is realized using [IR transmitters and recievers] (http://www.dx.com/p/mini-38khz-infrared-transmitter-ir-emitter-module-infrared-receiver-sensor-module-for-arduino-327293#.VuqPR_krKhc). We use transmitters and recievers prefabricated on tiny PCBs with three pins: ground (GND), positive supply voltage (VCC), and signal (SIG). They are rated to run on 3.3 - 5.5 volts, so they are pretty much "plug and play".
###"Bells & Whistles"
We wanted the cars to be a bit more fun, so we decided to add:
- Simple sound using a [piezoelectric speaker] (https://en.wikipedia.org/wiki/Piezoelectric_speaker) 
- RGB LEDs to provide lights beside the IR recievers to indicate hits and other things, 
- A [laser diode] (https://www.adafruit.com/products/1054) to work as a sight for the IR transmitter.

##Firmware

##Software




