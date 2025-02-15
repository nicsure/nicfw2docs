```c
typedef struct 
{
    u32 startFreq;
    u32 endFreq;
    u8 maxPower;
    struct
    {
        u8 txAllowed : 1;
        u8 wrap : 1;
        u8 modulation : 3;
        u8 bandwidth : 3;
    } bits;
} bandPlan;
```

Note: Bandplan comments are purely client-side.
