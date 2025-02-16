```c
typedef struct 
{
    u32 startFreq;
    u32 endFreq;
    u8 maxPower;
    struct
    {
        u8 txAllowed : 1; // bit 0 (mask 0x01)
        u8 wrap : 1; // bi1 1 (mask 0x02)
        u8 modulation : 3; // bit 2 to 4 (mask 0x1C)
        u8 bandwidth : 3; // bit 5 to 7 (mask 0xE0)
    } bits;
} bandPlan;
```

Note: Bandplan comments are purely client-side.
