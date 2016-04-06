#**Physical Web Toy**
##**Bluetooth Smart Controlled Car**

This project is a bachelor thesis done by four students of electrical engineering at [The Norwegian University of Science of Technology] (http://www.ntnu.edu/).
The project utilizes the [nRF52 Development Kit] (http://no.mouser.com/new/nordicsemiconductor/nordic-nrf52-dk/) (DK for short) from [Nordic Semiconductor] (http://www.nordicsemi.com/), which is a versatile single-board development kit for Bluetooth® Smart, ANT, and 2.4GHz proprietary applications. It is hardware-compatible with the Arduino Uno Revision 3 standard, so it  enables the use of 3rd-party shields that conform to this standard.

The thesis itself is scheduled to be delivered may 25th 2016, so the publication of this project is a snapshot of the functionality to coincide with the release of [The Physical Web] (https://google.github.io/physical-web/). Although the project is not fully completed, the main functonality is in place, and we are only missing simple lights and sound.

Our goal was to make several Bluetooth Smart controlled cars that can play a game of laser tag against each other as well as receive random power ups by driving over RFID tags.

The cars are built using a prefabricated kit designed for Arduino and other micro controllers. We are also in the process of making a body that goes over the cars using vacuum forming, and 3D-printing parts to attach various peripherals to the cars.

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

###Eddystone and Bluetooth Smart

The very heart in our DK, powered by [Eddystone] (https://en.wikipedia.org/wiki/Eddystone_(Google)) and Bluetooth® Smart, makes it possible to connect to our 4WD "Robot" Cars instantenously with almost any Bluetooth-device with access to the web. Thanks to Eddystone, our DK will act as a beacon, which in turn makes it a quick and easy process for connecting to the car. The DK will simply advertise our web-URL, and through our website you can control the car through any Bluetooth-device supporting Bluetooth® Smart. The DK itself runs a [very fast chip at 64MHz] (https://www.nordicsemi.com/Products/nRF52-Series-SoC), which makes our system very capable for this very use.

###Software Development Kit

Our project is based on Nordic Semiconductors Software Development Kit 11, and is a heavily modified version of ble_app_blinky and it's ble_lbs-service. The bluetooth service itself is greatly modified for our use, and consists of two characteristics; one used for read/write - used mainly by the software -, and one for notifications to the web from the firmware itself. 

Both characteristics consists of 20 bytes, and they both have several bytes which are not in use by the project at this moment. This makes our project flexible for further development.

###TWI Motor- and RFID-driver

In order for our DK to connect and communicate with our [motor] (https://learn.adafruit.com/adafruit-motor-shield-v2-for-arduino/overview)- and [RFID] (https://www.adafruit.com/products/789)-shields, we had to create designated drivers. These drivers were created purely on analyzing the logic on their first-party drivers made for Arduino, and although their design might be vastly different, our drivers work as intended. These drivers are made completely independent of our project, and can be used as libraries for other projects which utilizes these shields.

Note that the motordriver will always set new values for every motor, and currently there is no way of setting a motor value independently. Therefore, if there is a need to change only one motor value, you have to make certain that the old motor values is also defined.

Currently the RFID-driver does not have the option to read a RFID-tag or write to it; as this is not needed for our project. Our RFID-driver is designed to only notify the DK when a nearby tag is in the area.

###"Ouch! I've been hit!" also known as infrared receivers and emitters

Our DK has the ability to receive and send infrared signals, which is the staple function for creating the thrill of the hunt in our "lasertag"-game. The DK simply sends a high pulse to our emitter whenever the client has notified (via Bluetooth) the DK to shoot, and it recognizes a hit through Nordic Semiconductors very own GPIOTE driver. 

There is no other trick to it!

###RGB-LEDs and PWM.

In order to get more feedback from the game, we added some lights to our project. The DK communicates to these devices with a changing PWM-signal, and you guessed it, this is powered by Nordic Semiconductors app_pwm-library. It should be noted that we've added quite a lot upon this library to make it possible to not collide with our other timers, pins and Bluetooth-functionality. 

Right now you can call a simple function, set_rgb_color, to change the light to a set of predefined values. There is also room for defining further values.

##Software




