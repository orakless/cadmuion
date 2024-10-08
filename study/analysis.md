# cadmuion driver analysis
## Introduction
In order to do this analysis, I used **tshark** and **wireshark-qt** to inspect bytes of data received by my machine from the Huion Kamvas Pro 16 tablet.

The Huion Kamvas Pro 16 sends data packets to the computer like this:
- 64 bytes of data are reserved for the USB protocol  
- 12 bytes of data are for the actual data

## Decomposition of the data structure
### Universal
- **Byte 0**: data is inputted `0x08` or not `0x02`  
- **Byte 1**: type of data: button `0xe0`, touch-bar `0xf0`, stylus position without pressure `0x80`, stylus position with pressure information `0x81`
### Modes
#### Button [0xe0]
When you're in button mode (**Byte 1** == `0xe0`), **Byte 2** and **Byte 3** will always be the same (`0x0101`).  
**Byte 4** will be used to determine which button is pressed
| Button name* | Byte value (in hexadecimal) |
|--------------|-----------------------------|
| Button 1     | `0x01`                      |
| Button 2     | `0x02`                      |
| Button 3     | `0x04`                      |
| Button 4     | `0x08`                      |
| Button 5     | `0x10`                      |
| Button 6     | `0x20`                      |
| Logo button  | `0x40`                      |

*\*: It's not the official names. It's just numbered from top to bottom, with the Logo button being the last one, with a logo on it.*

#### Touchbar [`0xf0`]
When you're in touchbar mode, **Byte 2** and **Byte 3** will be the same as in button mode  
**Byte 4** will always be equal to `0x00`  
**Byte 5** will be the actual data, ranging from `0x01` to `0x08` (?, not sure, I'd need to check more)
#### Stylus  
##### Without pression [`0x80`]
**Byte 2** and **Byte 3**: horizontal position   
**Byte 4** and **Byte 5**: vertical position  
**Byte 10** and **Byte 11**: pen tilt
##### With pression [`0x81`]
**Byte 6** and **Byte 7**: pression, ranging from `0x0000` to `0xfff1`
