# cadmuion driver analysis
## Introduction
In order to do this analysis, I used tshark and wireshark-qt to inspect bytes of data received by my machine from the Huion Kamvas Pro 16 tablet.

The Huion Kamvas Pro 16 sends data packets to the computer like this :
- 64 bytes of data are reserved for the USB protocol
- 12 bytes of data are for the actual data

## Decomposition of the data structure
### Universal
- Byte 0: data is inputted (0x08) or not (0x02)
- Byte 1: type of data: button (0xe0), touchbar (0xf0), stylus position without pression (0x80), stylus position with pression information (0x81)
### Modes
#### Button [0xe0]
When you're in button mode (byte 1 == 0xe0), the byte 2 and byte 3 will always be the same (0x0101).
Byte 4 will be used to determine which button is pressed (one byte pos = one button, the first one is 0b00000001 and the last one is 0b01000000)
#### Touchbar [0xf0]
When you're in touchbar mode, the byte 2 and byte 3 will be the same as in button mode
Byte 4 will always be equal to 0x00 
Byte 5 will be the actual data, ranging from 0x01 to 0x08 (?)
#### Stylus  
##### Without pression (0x80)
Byte 2 and Byte 3: horizontal position 
Byte 4 and Byte 5: vertical position
Byte 10 and 11: pen tilt
##### With pression (0x81)
Byte 6 and 7: pression, ranging from 0x0000 to 0xfff1
