# Teleco-remotecontroller-rf-protocol
Frequency 868,25 mhz.  FSK 8 bytes

Start puls 2 mhz followed by 500us/1ms pulses. A "0" is represented by a 500 us (pos or neg). A "1" is represented by 1ms.

After each puls the signal is toggled. 1 500us pulse is given after the start puls.

Reading the signal from RFM22b in direct mode gives a bitstream as following:

1111 0110 0000 0101 1001 0000 0010 1010 0000 1011 0110 0000 1010 0101 0000 0100 1010 0000 0100 1100 0000 1101 0101 0000 
0101 0100 0000 1010 1011 0000 0101 0100

After decoding the bitstream has 8 bytes as following: A1,AC,B2,C0,B3,00,07,87

Bytes 1-5 will be the pairingcode. 

Byte 6 always 00. 

Byte 7: the key pressed. 

Byte 8: checksumsum 0x100- Sum Bytes 1..7

Byte 7 decoding

1=slats closed

2=slats full 

3=slats midposition

4=slats 45 degrees open

5= slats 1 step up

7=stop


8=slats 1 step down               
Paring code is devided in 3 parts 1101 1010 1001 1100 0011 0000 0001 0000 0100 0100
![afbeelding](https://github.com/user-attachments/assets/3c9eb5e5-f904-45ff-99ac-6c3ab7eac949)
Part 1: bits 0 to 13 (14 bits). 

Part 2 bits 18 to 28 (10 bits

Part 3 bits 30-39 (10 bits)

Bit 14,15,16,17 For group 1 for the controller bit 15 is set

Bit 28,29 always 0
