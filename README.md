# CANFDuino
![Image](https://i1.wp.com/togglebit.net/wp-content/uploads/2021/08/canfduino-front-open-enclosure_jpg_project-main.jpg?fit=749%2C421&ssl=1)

## Intro
CANFDuino is the best platform for open source CAN bus Arduino projects. It combines several essential features into one complete "ready-to-modify" package for the real world, including dual CANFD bus ports, SD card slot, and a multitude of analog/digital IO. It also includes a built-in prototyping shield for SMT and through-hole parts, and rugged packaging and connections.

## Hardware Hookups
For making connections the **hookup guide** is here: [https://togglebit.net/canfduino-hookup-guide/](https://togglebit.net/canfduino-hookup-guide/)

## Installing the Library and First Sketch
The following are steps for installing and testing CANFDuino support for the Arduino IDE, running and testing your first CAN sketch:

Note this content can also be found here [https://togglebit.net/getting-started-with-canfduino/](https://togglebit.net/getting-started-with-canfduino/)

**Step 1.** - Download the entie codebase, un-zip it. Rename "CANFDuino-master" to "CANFDuino".

**Step 2.** - Close the Arduino IDE if you have it open. Copy the whole "CANFDuino" folder to your local arduino hardware folder. The path for this hardware folder is: C:\Users\YOUR USERNAME\Documents\Arduino\hardware. The whole folder should be pasted directly into the hardware folder creating: C:\Users\YOUR USERNAME\Documents\Arduino\hardware\CANFDuino. If you do not have a hardware folder, create one and place CANFDuino folder inside of it.

**Step 3.** - Open the Arduino IDE, Go to the "Tools->Board->CANFDuino" to select the CANFDuino board as your target board.

**Step 4.** - Go to Tools->Board->-Boards Manager-Wait for the downloads to finish. Click on “Arduino SAMD Boards (32-bits ARM Cortex-M3), including Arduino M0” and Install. This can take a while to download if you don't already have the library.

**Step 5.** - Go to File->Examples->CANFDuino->CANFDuino_Test500Kb.ino (have a look at all the other examples as well)

**Step 6.** - Wire the CANbus DB9 connectors or jump the 2pin headers on the PCB to connect CAN0 bus to the CAN1 bus. CAN0 High->CAN1 High, CAN1 Low->CAN1 Low.

**Step 7.** - Jumper at least one of the two termination resistors on the PCB enabling termnination resistors on the bus. (two ideally)

**Step 8.** - Connect USB cable to CANFDuino and the PC, this will power the unit an prompt for a dirver install. Wait for the driver to install if needed (one option is to skip obtaining from windows and selecting the driver in the hardware\samd\drivers folder). Go to Tools->Port and select the COMM port that is labled CANFDuino. You may need to shut down the IDE and re-open it after installing the driver. (if needed, use FTDI exe in this repository to install driver)

**Step 9.** - Go to Sketch->Upload. After compiling the IDE will send a signal to the CANFDuino to enter the bootloader. Note when the unit is bootloading the orange LED will stay on and strobe off quickly for 1-2 seconds. This occurs anytime the processor is reset (initial power on etc). You should see the green/red comms LED's flicker during sketch upload.

**Step 10.** - Go to Tools->Serial Monitor, note that this will also cause entry into bootloader for about 4-5 seconds before the sketch runs (for permanant code/bootloader bypass there is a jumper on the PCB).

**Step 11.** - See diagnostic printout in the comms window indicating pass/fail of testing. Typically the cause of failure is improper wiring or no termination resistor.

# Example Code
## CANFduino_CANTerm.ino - CAN/CANFD Packet Monitoring
![Image](https://togglebit.net/wp-content/uploads/2021/11/ezgif.com-gif-maker-1.gif)

**CANTerm** is a cheapo 2 port CAN/CANFD packet monitor that can be used in simple serial terminal programs regardless of OS without special PC software. The CANFDuino is used to print packet payloads to the screen using terminal commands in static locations for easy viewing. Supports multiple CAN and CANFD baud rates, ID range filtering and stores the last settings into flash.

CANTerm has been tested with the PuTTY terminal program (2MBaud). PuTTY is probably the most flexible terminal tool for Windows, where the font size, number of lines, size of display can be configured to pack more data on one screen. [https://www.chiark.greenend.org.uk/~sgtatham/putty/](https://www.chiark.greenend.org.uk/~sgtatham/putty/)


Monitor CAN bus traffic in the comfort of a Chrome browser with serial terminal from google's chromelabs (2MBaud). Just click the link below and connect to the COM port.
[https://googlechromelabs.github.io/serial-terminal/](https://googlechromelabs.github.io/serial-terminal/)

Note: The Arduino Serial Monitor is not a terminal program and will not work.

**10 Steps for using CANTerm**

**Step 1.** - Do everything above in getting started section to make sure yoru hardware works. Hook up the CAN/FD ports to the buss(es) you want to monitor. 

**Step 2.** - Open ArduinoIDE, go to File->Examples->CANFDuino_CANTerm.ino

**Step 3.** - Upload it with the IDE.

**Step 4.** - Open up your favorite terminal program (PuTTY, or click the chromelabs serial terminal link above), select the baud rate to 2MBaud, select the correct COM port and click connect. Wait for a few seconds for on-screen instructions to appear and follow them (if no on-screen instructions, check the baud rate and COM port setting and connect again).

**Step 5.** - Select if you want CAN0 or CAN1 or both ports to do packet monitoring.

**Step 6.** - Select the baud rate for CAN packets (or none)

**Step 7.** - Select the baud rate for CANFD packets (or none)

**Step 8.** - Type in the hex ID for the LOWEST ID you want to monitor (or type ALL for all packets to be monitored)

**Step 9.** - Type in the hex ID for the HIGHEST ID you want to monitor. All ID's between these ranges will pass through

**Step 10.** - If you selected 2 ports to monitor....you will be asked to repeat settings for the second port. Watch for traffic to hit the screen.

Note: if you do not want to wait the several seconds between power cycles or new terminal connections use the bootloader bypass jumper detailed in the hookup guide.


## CANFDuino_OBD2Logger.ino - OBD2 datalogger to SD Card

This is an example sketch that uses and OBD2 library to continously poll the OBD2 port of a vehicle and log the resulting data in CSV format to the SD card of the CANFDuino. The data is also printed to the serial monitor (LogScreen). This particular example polls for RPM, Speed, Throttle Position, Coolant Temp, Engine Load, Mass Airflow Rate, and Intake Air Temp. More data can be obtained by adding an **cOBDParameter** object for the item of interest, OBD2 is well documented a reference of parameters can be found here [https://en.wikipedia.org/wiki/OBD-II_PIDs] (https://en.wikipedia.org/wiki/OBD-II_PIDs). Have a look at the OBD2.h file and the parameters given in the example to get an idea how to properly add a new parameter (you will also need to modify the LogScreen, WriteOBD2header and logOBDData functions). **Note** that if you are using an OBD2 to DB9 cable, you need to watch the pinout of the DB9 as the CANFDuino uses 2,7 and 3 (see hookup guide), many OBD2 to DB9 cables use pins 3,5 and 1. 

