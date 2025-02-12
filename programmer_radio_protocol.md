# Programmer-Radio Communication Protocol Documentation

## Overview

Communication between the Programmer and the Radio occurs over a serial link. This link can be either wired or via Bluetooth Low Energy (BLE). The communication follows a simple packet-based protocol.

### Connection Types

- **Wired Serial**: 38400 baud, 8 Data Bits, No Parity, 1 Stop Bit
- **Bluetooth LE**: 9600 Baud

## Protocol Structure

The protocol uses a packet-based system where each packet starts with a single ID byte. Packets can vary in length, and some packets consist of only a single byte.

## EEPROM Read/Write Operations

Packets with IDs `0x30` and `0x31` are used for reading from and writing to the EEPROM. The EEPROM stores all configuration data for the radio. The layout of the EEPROM is detailed in a separate document.

### Read EEPROM - Packet ID `0x30`

A read request consists of a two-byte packet:

```c
struct {
    uint8_t packetID; // 0x30
    uint8_t blockNum; // EEPROM address divided by 32
} eepromReadReq; // Sent from Host to Radio
```

- The EEPROM has **8 KB of storage** (addresses `0x0000` to `0x1FFF`).
- It is divided into **256 blocks**, each containing **32 bytes**.
- The `blockNum` field specifies which block of 32 bytes to read.

The Radio responds to a read request with the following structure:

```c
struct {
    uint8_t packetID; // 0x30
    uint8_t data[32]; // EEPROM data block
    uint8_t checksum; // Additive checksum of data[]
} eepromReadReply; // Sent from Radio to Host
```

### Write EEPROM - Packet ID `0x31`

A write request consists of the following structure:

```c
struct {
    uint8_t packetID; // 0x31
    uint8_t blockNum; // EEPROM address divided by 32
    uint8_t data[32]; // New EEPROM data
    uint8_t checksum; // Additive checksum of EEPROM data
} eepromWriteReq; // Sent from Host to Radio
```

#### Write Acknowledgment

- A valid EEPROM write request will be acknowledged by a **single-byte response**: `0x31`.
- An **invalid write request** will be ignored **without any acknowledgment**.

## Other Serial Commands

### NOP - Packet ID `0x00`

- Does nothing, primarily used to keep the communication link active (both directions).

### Ping - Packet IDs `0x01` & `0x02`

- Sending a single-byte ping packet to the radio will cause the radio to respond with the same packet.
- Useful as a quick check to ensure the radio is responding.

### Remote KeyPad Action - Packet ID `0x4C`

```c
struct {
    uint8_t packetID; // 0x4C
    uint8_t qualifier; // 0x4D
    uint8_t keypadID; // 0x80 to 0x93 and 0xFF
} remoteKeyReq; // Sent from Host to Radio
```

- Simulates a keypad button press on the radio.
- Special key value `0xFF` is used to release the key press.
- Only one key press can be processed at a time.

#### Keypad IDs:

- `0x80-0x89` = Keys 0 to 9
- `0x8A` = Menu Key
- `0x8B` = Up Key
- `0x8C` = Down Key
- `0x8D` = Exit Key
- `0x8E` = `*` Key
- `0x8F` = `#` Key
- `0x90` = PTT-A Key
- `0x91` = PTT-B Key
- `0x92` = Flashlight Key
- `0x93` = External PTT Key (external microphone PTT)
- `0xFF` = No Key (used to release a key)

### Reboot Radio - Packet ID `0x49`

- Performs a soft reset of the radio.
- Should be sent after any EEPROM write operation.
- No acknowledgment is returned.

### Request Scan Ignore List - Packet ID `0x43`

```c
struct {
    uint8_t packetID; // 0x43
    uint32_t frequencies[50]; // little-endian, frequencies are in 10 Hz units.
} ignoredFreqReply; // Sent from Radio to Host
```

- Requests a list of frequencies currently being ignored during scanning.

### Disable Radio - Packet ID `0x45`

- Disables the radio chip SoC.
- Should be sent before EEPROM read/write operations to prevent interference.
- The radio will acknowledge with `0x45`.

### Enable Radio - Packet ID `0x46`

- Enables the radio chip SoC.
- Should be sent after EEPROM reads to resume normal operation.
- The radio will acknowledge with `0x46`.

### Turn Remote Control On - Packet ID `0x4A`

- Enables the Remote Control system.
- Covered in another document.
- The radio will acknowledge with `0x4A`.

### Turn Remote Control Off - Packet ID `0x4B`

- Disables the Remote Control system.
- Covered in another document.
- The radio will acknowledge with `0x4B`.

## Summary

This protocol enables efficient communication between the Programmer and the Radio, supporting both wired and Bluetooth LE connections. Various packet-based commands facilitate EEPROM read/write operations, remote control functions, and radio status management.
