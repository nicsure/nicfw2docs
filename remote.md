# Remote Protocol

## Overview
The remote protocol is an extension of the existing programmer/radio communication protocol. Please review that protocol first before proceeding.

## Remote Mode Activation
To activate remote mode on the radio, send a single-byte packet with ID `0x4A`. To deactivate remote mode, send `0x4B`. Upon receiving either of these packets, the radio will acknowledge by echoing back the same ID byte to the host.

---

## Radio to Host Packets
The following packets are sent from the radio to the host:

### Display Text (Packet ID: `0x64`)
```c
struct {
    u8 ID;       // 0x64
    u8 fontSize;
    u8 x;        // Location
    u8 y;
    u16 fgColor; // Foreground color (see Color Encoding below)
    u16 bgColor; // Background color
    char[] text; // Null-terminated variable-length string
} displayTextPacket;
```

### Draw Rectangle (Packet ID: `0x65`)
```c
struct {
    u8 ID;       // 0x65
    u8 x;        // Location
    u8 y;
    u8 width;
    u8 height;
    u16 color;   // Color (see Color Encoding below)
} drawRectanglePacket;
```

### Draw Custom Symbol (Packet ID: `0x66`)
```c
struct {
    u8 ID;       // 0x66
    u8 symbolID;
    u8 x;        // Location
    u8 y;
    u16 fgColor; // Foreground color (see Color Encoding below)
    u16 bgColor; // Background color
} drawSymbolPacket;
```

#### Custom Symbol IDs
| Symbol ID | Description             |
|-----------|-------------------------|
| `0x00`    | Dual Watch Icon         |
| `0x01`    | Busy Lock Icon          |
| `0x02`    | Music Note (not used)   |
| `0x03`    | Right Arrow             |
| `0x04`    | Up Arrow                |
| `0x05`    | Down Arrow              |
| `0x06`    | VOX Icon                |
| `0x07`    | Keylock Icon            |
| `0x08`    | Scan Icon               |
| `0x09`    | Long Press Shift Icon   |
| `0x0A`    | NO                      |
| `0x0B`    | AA (forms NOAA icon)    |
| `0x0D`    | Bluetooth Icon          |
| `0x0E`    | Blank Symbol (for erasing) |
| `0x0F`    | Pause Icon              |

### Signal Strength (Packet ID: `0x67`)
```c
struct {
    u8 ID;              // 0x67
    u8 signalStrength;  // Range: 0-120 (values >120 capped to 120)
    u8 mode;            // 0=Rx, 1=Tx
} signalStrengthPacket;
```

### External Noise Level (Packet ID: `0x68`)
```c
struct {
    u8 ID;         // 0x68
    u8 noiseLevel; // Range: 0-120 (values >120 capped to 120)
    u8 mode;       // 0=Rx, 1=Tx (tx mod meter)
} noiseLevelPacket;
```

### Signal Bar Vertical Position (Packet ID: `0x69`)
```c
struct {
    u8 ID;      // 0x69
    u8 sigBarY; // Vertical position of the signal bar
} signalBarPosPacket;
```

### LED Status (Packet IDs: `0x70` - `0x7F`)
These single-byte packets control LED status. The lower 4 bits represent LED states:
- **Bit 0** = Left Green LED
- **Bit 1** = Left Red LED
- **Bit 2** = Right Green LED
- **Bit 3** = Right Red LED
- `1 = ON`, `0 = OFF`

---

## Color Encoding
Colors are encoded as **16-bit values** using the **BGR 565 format**:
```
Bit: FEDCBA9876543210
     BBBBBGGGGGGRRRRR
```
- **B (Blue)**: 5 bits
- **G (Green)**: 6 bits
- **R (Red)**: 5 bits

---

## Host to Radio Packets
The host sends **single-byte packets** that represent keypad presses. The radio responds as if a physical key was pressed or released.

### Key Press Events (Packet IDs: `0x80` - `0x93`)
| Packet ID | Key |
|-----------|-----|
| `0x80` - `0x89` | Keys 0 - 9 |
| `0x8A` | Menu Key |
| `0x8B` | Up Key |
| `0x8C` | Down Key |
| `0x8D` | Exit Key |
| `0x8E` | * Key |
| `0x8F` | # Key |
| `0x90` | PTT-A Key |
| `0x91` | PTT-B Key |
| `0x92` | PTT-E Key (external mic PTT) |
| `0x93` | Flashlight Key |

### Key Release Event (Packet ID: `0xFF`)
- `0xFF` = No Key (release)
- The radio only processes one key press at a time.

---

## Notes
- The remote control operates as an **extension of the main programmer/radio protocol**.
- The radio acknowledges activation (`0x4A`) and deactivation (`0x4B`) requests by echoing them back.
- The radio can only register **one key press at a time**.

This structured format ensures clarity and easy reference for developers implementing or integrating the remote protocol.
