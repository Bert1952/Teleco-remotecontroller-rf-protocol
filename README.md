# Teleco-remotecontroller-rf-protocol for adjustable slats unit TVPLD868C150TT integration with android app via a second remote unit
![afbeelding](https://github.com/user-attachments/assets/73f1b692-a53e-4310-b1d7-6d52b4c2b608)

Frequency 868,25 mhz.  FSK 8 bytes <br/>
Start puls 2 mhz followed by 500us/1ms pulses. A "0" is represented by a 500 us (pos or neg). A "1" is represented by 1ms. 
![DatastreamTeleco](https://github.com/user-attachments/assets/c8981cc3-0ee0-4e4a-a506-624d7440a603)

After each puls the signal is toggled. 1 500us pulse is given after the start puls. <br/>
Reading the signal from RFM22b in direct mode gives a bitstream as following:<br/>
1111 0110 0000 0101 1001 0000 0010 1010 0000 1011 0110 0000 1010 0101 0000 0100 1010 0000 0100 1100 0000 1101 0101 0000 
0101 0100 0000 1010 1011 0000 0101 0100<br/>
After decoding the bitstream has 8 bytes as following: A1,AC,B2,C0,B3,00,07,87<br/>
Bytes 1-5 will be the pairingcode.<br/>
Byte 6 always 00. <br/>
Byte 7: the key pressed. <br/>
Byte 8: checksumsum 0x100- Sum Bytes 1..7<br/>
Byte 7 decoding<br/>
1=slats closed<br/>
2=slats full <br/>
3=slats midposition<br/>
4=slats 45 degrees open<br/>
5= slats 1 step up<br/>
7=stop<br/>
8=slats 1 step down<br/>               
Paring code is devided in 3 parts<br/>
![afbeelding](https://github.com/user-attachments/assets/3c9eb5e5-f904-45ff-99ac-6c3ab7eac949)<br/>
Part 1: bits 0 to 13 (14 bits). <br/>
Part 2 bits 18 to 28 (10 bits<br/>
Part 3 bits 30-39 (10 bits)<br/>
Bit 14,15,16,17 For group 1 for the controller bit 15 is set<br/>
Bit 28,29 always 0<br/>
The numbers are quasi random, but analysis in exel shows that there are links between the button pressed and the bit patterns generated.There are 6 more bits reserved which are apparently used for other groups of the remote control. (6 groups of 7 switches=total of 42 codes)<br/>

Since it is of no further interest to examine the coding, we use the codes as received. The system is not Keelloq coding. A received code word can be used again. The motor control receiver can simply be learned that the ESP generated signals should be accepted as new contoller.<br/>

In B4r, an encoder was created that then makes the RFM22b send a signal to which the sunroof controller responds.<br/>

In b4a, an app was created that controls the esp8266 via MQTT. If the messages run through the public channel, another user could also control the screen. Therefore, a check is built in.<br/>
The app sends a command via Mqtt. The esp then sends a random number back to the app. The latter makes a calculation based on a given Hash and sends the answer back to the esp. The esp compares the calculation with the internal calculation, and when the result is the same, the signal is sent to the sunscreen. A message is then also sent back to the app so the user gets a notification of accepted.<br/>
attached: B4a example program. You need to specifiy your MQTT provider creditals. In case you are not on a public channel the hash control can be removed.<br/>
B4r program for the ESP8266 D1 mini clone. Schematic how to connect the ESP to the RFM22b. (note: RFM22b is a old chip from Hope. You can replace it by a modern type, but some rework on the programming should be done)<br/>

![Screen Shot 09-06-24 at 08 05 PM](https://github.com/user-attachments/assets/8b8a93eb-a344-40c7-8141-b636120413e3)
