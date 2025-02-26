# EEPROM Layout

The EEPROM has 8k of space addressed from `0x0000` to `0x1FFF`. 
All multi-byte values are LITTLE-Endian for V2.0X codeplugs.
All multi-byte values are BIG-Endian for V2.5X codeplugs.

For details on data types, consult the data structure document for the data type.

## Layout

```c
struct {
    channelInfo vfoA; // 0x0000
    channelInfo vfoB; // 0x0020
    channelInfo memoryChannels[198]; // 0x0040
    settingsBlock; // 0x1900
    ...
    uint16_t bandplanMagic; // 0x1A00, value = 0xA46D
    bandPlan bandPlans[20]; // 0x1A02
    ...
    scanPreset scanPresets[20]; // 0x1B00
    ...
    char[6] groupLabels[16]; // 0x1C90 (null terminated)
    struct { // 0x1CF0
        dtmfSequence sequence;
        char sequenceLabel[8]; // null terminated
    } dtmfPresets[20];
    ...
    int8_t xtal671; // 0x1DFB
    uint8_t maxPowerWattsUHF; // 0x1DFC (0.1W units)
    uint8_t maxPowerSettingUHF; // 0x1DFD
    uint8_t maxPowerWattsVHF; // 0x1DFE (0.1W units)
    uint8_t maxPowerSettingVHF; // 0x1DFF
    uint8_t powerTableVHF[256]; // 0x1E00 (first byte is magic 0x57)
    uint8_t powerTableUHF[256]; // 0x1F00 (first byte is magic 0xD1)
} eepromLayout;
