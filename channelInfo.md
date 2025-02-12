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
            u16 g0 : 4;
            u16 g1 : 4;
            u16 g2 : 4;
            u16 g3 : 4;
        } single;
    } groups;
    struct
    {
        u8 bandwidth : 1;
        u8 modulation : 2;
        u8 position : 1;
        u8 pttID : 2;
        u8 reversed : 1;
        u8 busyLock : 1;
    } bits;
    char reserved[4];
    char name[12];
} channelInfo;
