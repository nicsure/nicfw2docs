```c
typedef struct 
{
    U32 rxFreq;
    U32 txFreq;
    U16 rxSubTone; // subtone is CTCSS if bit 15 & 14 is not set 0.1Hz units
    U16 txSubTone; // subtone is DCS if bit 15 is set and inverted if bit 14 is set.
    u8 txPower;
    union
    {
        u16 value;
        struct
        {
            u16 g0 : 4; // (mask 0x000F)
            u16 g1 : 4; // (mask 0x00F0)
            u16 g2 : 4; // (mask 0x0F00)
            u16 g3 : 4; // (mask 0xF000)
        } single;
    } groups;
    struct
    {
        u8 bandwidth : 1; // bit 0 (mask 0x01)
        u8 modulation : 2; // bits 1 to 2 (mask 0x06)
        u8 position : 1; // bit 3 (mask 0x08)
        u8 pttID : 2; // bits 4 to 5 (mask 0x30)
        u8 reversed : 1; // bit 6 (mask 0x40)
        u8 busyLock : 1; // bit 7 (mask 0x80)
    } bits;
    char reserved[4];
    char name[12];
} channelInfo;
