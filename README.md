# Mahmood-PTZKeyboard
This project which will automate the process of managing various professional cameras. Each system will consist of multiple camera heads that will be controlled by a main server. It will also consist of modules to take input from user.


## Design Requirements
The design will consist primarily of three PCB boards. For easiness, naming convention is listed as:

1. Controller Board
2. Module Board / Boards
3. Camera Head Board

### Camera Controller Board
#### General Description
The Controller Board is the also the Main Server in the mechanism. It will feature and LCD with which the user can interact with the cameras spread across and also be able to observe the different parameters and positions of the cameras. The screen will be the main HUB for all the data and activity. The screen will host an Android OS and load an Android application for User Interaction with the product. The Controller board will feature an RK3399 based design as the main brains and a powerful WiFi transceiver (non ESP32) for all the wireless communication. Orange Pi 4B can serve as a great reference for the overall schematic and PCB design of the **Camera Controller Board**

#### Key Points for design scheme
1. It features a touch LCD for display and interaction. 
2. It will feature a high end processor or SoM (depending on the size of the board) that will handle most of the processing such as data received from all the nodes (module board or camera head boards) as well as interaction with Touch LCD. The processor will further be capable of booting an Android OS and run a full fledged application.
3. The controller board will further have an on-board high power WiFi processor to allow communication with multiple slaves. For this, two versions will be made. The first version will be made with a single high power WiFi chip in mind whereas the second chip will use 2 x ESP32 SoCs or modules with their own antennae to allow for connection of multiple slaves with the Main Master.
4. PCB dimensions are not yet finalized so we're open to design and select components as we like.
5. The controller board will be powered directly using line voltage and will have the option of a backup battery as well. The battery selected will be 2 x LiPo or LiIon (8.4V input).
6. The board will have an on-board charge circuit as well to allow for battery charging when the device is running on direct line voltage.
7. The charge and discharge time of battery is not yet decided. We will select such that it should be able to give a backup of about 5-6hrs and be able to get fully charged in about 2-3hrs.
8. The device should also be capable of charging other module boards or exchange data with them if the module boards are connected directly.
9. Depending on the LCD selected, we may have 12V as the maximum voltage in the design and thus we can easily power this thing using a 12V USB-C adapter that will also serve the purpose of charging the LiPo/LiIon battery.
10. It is also recommended to provide bridges between all the power inputs and outputs for prototype phase as well as programming connections for RK3399 and the WiFi chip involved in the board.
11. Test points for all communication lines such as UART / I2C / SPI / LCD Interface / etc should also be provided as well as test points for voltage signals.
12. The board should also have the Pogo connectors for attaching with the Module board and charge its battery. When in direct contact mode, the board will start receiving information from the Module Board over Serial Interface (preferably UART) instead of the WiFi interface.

### Camera Head Board
#### General Description
The Camera Head Board is responsible for interfacing with the Cameras and controlling them. It is also responsible for driving the different motors present on the Camera via Motor Driver circuitry. For this purpose, 2 x ESP32 will be used where one ESP32 will serve as the WiFi transceiver and the other ESP32 will serve as the motor driver MCU. There will be a main MPU present on the board consisting of AM335x (BeagleBone Black processor) that will boot up linux to be able to load drivers for the camera and communicate with it.

#### Key Points for design scheme
1. The design will be battery or adapter powered. Max Voltage is 36V and Max current maybe 7A (needs to be confirmed)
2. There will 3 x Processing units present on this board. First is the Beaglebone AM335x processor, the second is the ESP32 as WiFi transceiver and the third one is ESP32 for motor drive over SPI Interface.
3. The motor driving circuitry will consist of 2 BLDC motor drivers from Trinamic namely **TMC6100 as the driver and TMC4671 as the brain to run the hardware**. The intent for using 2 chips for BLDC motor drive is so that an external motion controller the TMC4671 can report all the status and information to the MCU about the motor over SPI reducing overhead for the MCU.
4. The voltage levels for this board can go up to is 36V that will be fed to the motors and the current consumption will depend majorly on what each motor can draw plus the rest of the circuitry. All of this must be considered before designing the power regulation block.
5. It is also recommended to provide some form of isolation between the motor supply and the rest of the circuitry. We can leave the isolation of digital interface in this case.
6. The MPU (AM335x) will communicate with the ESP32 (Motor Driver) over a serial interface preferably UART Interface. The ESP32 responsible for WiFi transmission will be connected to the BeagleBone (AM335x) over another UART Inteface. Hence, the chain will mean that the BeagleBone MPU will sit at the heart of the PCB and will be communication with 2 x ESP32s over UART Interface.
7. **Reseach and brainstorming needed here**: The client wants to have a small backup battery that can at least keep the system running when the Main battery dries out. Since the Main battery is not rechargeable, while the user is swapping the battery, this backup battery must power the whole system just like a UPS does for at least 1 minute. We need to find a battery and a corresponding charge circuitry that can fulfill this requirement.
8. It is also recommended to provide bridges between all the power inputs and outputs for prototype phase as well as programming connections for all three MCUs / MPU involved in the board.
9. Test points for all communication lines such as UART / I2C / SPI / etc should also be provided as well as test points for voltage signals.

### Module Board
#### General Description
The module board will house a rotating knob that can change the different camera configurations such as PAN, Tilt, Zoom etc depending on what kind of module board is in operation. It is going to be a simple board that will only send the PAN / Tilt etc info to the Camera Controller Board informing the Server what position / focus the user wants. The board will be powered via battery and will be able to charge its battery when mounted onto the Camera Controller Board via Pogo pins. The board will function in two modes:
1. Direct contact mode: In direct contact mode, the board will send all the data to Controller Board over Serial interface and will also charge its own battery through charging circuitry present on the module board itself.
2. Wireless Mode: There will be no charging, the module board will be solely powered over the battery and will be communicating with the Controller board over WiFi interface

#### Key Points for design scheme
1. The board will be powered using a single LiPo/LiIon cell.
2. The board will have the charge circuitry present on it.
3. The max voltage may be 12V and for this, the battery voltage will be boosted by a circuitry.
4. The major components on this will be a BLDC motor of 12V and an encoder cictuitry and corresponding driver circuitry
5. There will be 1 x ESP32 present for hardware interface as well as communication on the board.
6. We may also need to give some LEDs on the board for a nice look but the amount of LEDs will be specified later
7. The board will also have Pogo connectors for attaching directly to the Controller board as specified in **General Description**.
8. It is also recommended to provide bridges between all the power inputs and outputs for prototype phase as well as programming connections for ESP32.
9. Test points for all communication lines such as UART / I2C / SPI / etc should also be provided as well as test points for voltage signals.
