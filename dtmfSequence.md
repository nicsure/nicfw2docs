```c
typedef struct {
    struct {
        u32 length : 4; // byte 0
        u32 d0 : 4;
        u32 d1 : 4;     // byte 1
        u32 d2 : 4;
        u32 d3 : 4;     // byte 2
        u32 d4 : 4;
        u32 d5 : 4;     // byte 3
        u32 d6 : 4;
    } first;
    struct {
        u8 d7 : 4;      // byte 4
        u8 d8 : 4;
    } second;
} dtmfSequence;
