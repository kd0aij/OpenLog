Connecting Up to update the OpenLog Firmware
============================================

Objective:
To update the OpenLog Firmware to make it suitable for use with MAVLink.

There are 3 benefits:
1. Making the OpenLog binary safe for MAVLink.
2. Improving the Baud Rate reception at 57600 
3. Imroving the general speed at which the OpenLog can receive characters 

Overview of the steps:
1. Set up an interactive session with OpenLog to check that your serial connection works.
2. Download and prepare the update firmware and DOS command for updating the Open
3. Initiate the bootloader, and download the firmware
4. Change the number of escape characters to zero in CONFIG.TXt to make your device binary safe for MAVLink.

What you will need this for this update:-
1. A PC connected to the Internet
2. Some type of serial Terminal emulator for interacting with the OpenLog
2. An SDRAM memory reader / writer for the SDRAM chip in the OpenLog
3. An OpenLog (obviously)
4. A USB FTDI board from Sparkfun or an equivalent.

There is a video demonstration of this update procedure at:-
http://vimeo.com/44476888

The OpenLog product page is at:-
http://www.sparkfun.com/products/9530

Step 1:- Interactive session with the OpenLog to check connectivity

I connect the OpenLog to my PC using a USB FTDI device
Here is a web page showing such a device:-
http://www.sparkfun.com/products/718

Disconnect your USB FTDI board from the PC (i.e. power it down).


4 wires are required to be connected.
OpenLog Ground to to FTDI Ground
VCC of OpenLOG to FTDI VCC on side pin 
TX on OpenLog to RX on FTDI
RX on OpenLog to TX on FTDI

Put your SD RAM chip in your memory reader, and check the configuration settting by reading CONFIG.TXT in the main folder.

For firmware of the 2.X releases this may look something like:-

For firmware on the OpenLog of the 3.X release this may look something like:-
57600,26,3,0,1,1
baud,escape,esc#,mode,verb,echo

My Baud Rate is set to 57600. The more usual rate is 19200.

You will now want to estabilsh the COM port number of your FTDI / USB connection to the OpenLog.
Open your Control Panel on your PC, and select "Hardware and Sound" / "View Devices and Printers"
Make a visual note of the devices that are currently on screen. You will be looking for a new device in a moment.

With your wiring in place from the FTDI to the Openlog, now connect your USB FTDI to your PC to power up the OpenLog.

A new device should appear in your control panel. e.g. FT232R USB. Right clock on that device and select Properties.
Click on the Hardware Tab, and you should be able to see which COM device the FTDI USB is connected to.

Connect to the FTDI / OpenLog from your terminal emulator using the appropriate device, For Example COM5 and also make sure you
select the correct baud rate. This will be the baud rate that was set in your CONFIG.TXT file. e.g. 57600

Now start typing in the transmission window of your terminal emaulator and Watch for the blue light illuminating on
the OpenLog. If it lights up each time you type characters, then it is at least receiving a transmission. (Note: The blue
light will inidicate any activity in terms of receiving characters even if the baud rate is wrong).

Press the number of escape character (e.g. control-z) that was set in your config.txt file. (in my case I had it set to 9)
On pressing my control-Z for the 10th time, the  ">" symbol was sent by OpenLog to my terminal emulator window. That is
the OpenLog command prompt. I entered the command "help" and a menu of help items appeared on my screen. So now I was sure
that my serial communications were working with the OpenLog. I hope that you too can get to the same point.

It is now vital that you disconnect your terminal emulator so that the COM device is released and available for the 
firmware update tool called avrdude.exe .

From the unzipped directory of files, run the DOS command below, changing the COM port to reflect the COM port that you are using.
Note: The baud rate for this command will always be 57600 regardless of whatever Baud rate was in CONFIG.TXT in the Openlog
SD RAM file sysetm. This is because the avrdude.exe program is using the bootloader in the microprocessor chip which is permanently
set to 57600.

Afterwards. Look again interactively.
You should still be able to escape, because your escape character is not set to zero.
Now set your CONFIG.TXT with zero escape characters with a line like:-
57600,26,0,0,1,1
baud,escape,esc#,mode,verb,echo
You should now find that when using the terminal, no amount of escape characters (control-z in my case),
will escape the file capture session out to an interactive session.

All Done !


== A FAILED SESSION BECAUSE POWERED UP THE OPENLOG TOO SLOWLY AFTER STARTING AVRDUDE ===
C:\Users\phollands\Desktop\OpenLog Firmware>avrdude.exe -p atmega328p -P COM5 -c
 stk500v1 -b 57600 -Cavrdude.conf -U flash:w:openlog_binary_safe_3_1_1.cpp.hex
avrdude.exe: stk500_getsync(): not in sync: resp=0x30
avrdude.exe: stk500_disable(): protocol error, expect=0x14, resp=0x51

avrdude.exe done.  Thank you.
================================================================================

============================     A SUCCESSFUL SESSION ==========================

C:\Users\phollands\Desktop\OpenLog Firmware>avrdude.exe -p atmega328p -P COM5 -c
 stk500v1 -b 57600 -Cavrdude.conf -U flash:w:openlog_binary_safe_3_1_1.cpp.hex

avrdude.exe: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.02s

avrdude.exe: Device signature = 0x1e950f
avrdude.exe: NOTE: FLASH memory has been specified, an erase cycle will be perfo
rmed
             To disable this feature, specify the -D option.
avrdude.exe: erasing chip
avrdude.exe: reading input file "openlog_binary_safe_3_1_1.cpp.hex"
avrdude.exe: input file openlog_binary_safe_3_1_1.cpp.hex auto detected as Intel
 Hex
avrdude.exe: writing flash (28834 bytes):

Writing | ################################################## | 100% 14.43s

avrdude.exe: 28834 bytes of flash written
avrdude.exe: verifying flash memory against openlog_binary_safe_3_1_1.cpp.hex:
avrdude.exe: load data flash data from input file openlog_binary_safe_3_1_1.cpp.
hex:
avrdude.exe: input file openlog_binary_safe_3_1_1.cpp.hex auto detected as Intel
 Hex
avrdude.exe: input file openlog_binary_safe_3_1_1.cpp.hex contains 28834 bytes
avrdude.exe: reading on-chip flash data:

Reading | ################################################## | 100% 12.17s

avrdude.exe: verifying ...
avrdude.exe: 28834 bytes of flash verified

avrdude.exe: safemode: Fuses OK

avrdude.exe done.  Thank you.


C:\Users\phollands\Desktop\OpenLog Firmware>





