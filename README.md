# Mahmood-PTZKeyboard
This project which will automate the process of managing various professional cameras. Each system will consist of multiple camera heads that will be controlled by a main server. It will also consist of modules to take input from user.


## Design Requirements
The design will consist primarily of three PCB boards. For easiness, naming convention is listed as:

1. Controller Board
2. Module Board / Boards
3. Camera Head Board

### Controller Board
The Controller Board is the also the Main Server in the mechanism. The Controller Board has the following main features:

1. It features a touch LCD for display and interaction. 
2. It will feature a high end processor or SoM (depending on the size of the board) that will handle most of the processing such as data received from all the nodes (module board or camera head boards) as well as interaction with Touch LCD. The processor will further be capable of booting an Android OS and run a full fledged application.
3. The controller board will further have an on-board high power WiFi processor to allow communication with multiple slaves. For this, two versions will be made. The first version will be made with a single high power WiFi chip in mind whereas the second chip will use 2 x ESP32 SoCs or modules with their own antennae to allow for connection of multiple slaves with the Main Master.
4. PCB dimensions are not yet finalized so we're open to design and select components as we like.
5. The controller board will be powered directly using line voltage and will have the option of a backup battery as well. The battery selected will be 2 x LiPo or LiIon (8.4V input).
6. The board will have an on-board charge circuit as well to allow for battery charging when the device is running on direct line voltage.
7. The charge and discharge time of battery is not yet decided. We will select such that it should be able to give a backup of about 5-6hrs and be able to get fully charged in about 2-3hrs.
8. The device should also be capable of charging other module boards or exchange data with them if the module boards are connected directly.