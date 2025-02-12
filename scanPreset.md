```c
typedef struct 
{
    U32 startFreq;
    u16 range;
    u16 step;
    u8 resume;
    u8 persist;
    struct {
        u8 modulation : 2;
        u8 ultrascan : 6;
    } bits;
    char label[9];
} scanPreset;
