```c
typedef struct {
    u16 magic;                  // 0x00
    u8 squelch;                 // 0x02
    u8 dualWatch;               // 0x03
    u8 autoFloor;               // 0x04
    u8 activeVfo;               // 0x05
    u16 step;                   // 0x06
    u16 rxSplit;                // 0x08
    u16 txSplit;                // 0x0a
    u8 pttMode;                 // 0x0c
    u8 txModMeter;              // 0x0d
    u8 micGain;                 // 0x0e
    u8 txDeviation;             // 0x0f
    s8 xtal671;                 // 0x10
    u8 battStyle;               // 0x11
    u16 scanRange;              // 0x12      
    U16 scanPersist;            // 0x14
    u8 scanResume;              // 0x16
    u8 ultraScan;               // 0x17
    u8 toneMonitor;             // 0x18
    u8 lcdBrightness;           // 0x19
    u8 lcdTimeout;              // 0x1a
    u8 breathe;                 // 0x1b
    u8 dtmfDev;                 // 0x1c
    u8 gamma;                   // 0x1d
    u16 repeaterTone;           // 0x1e
    struct {                    // 0x20
        u8 group;
        u8 lastGroup;
        u8 groupModeChannels[16];
        u8 mode;
    } vfoState[2];
    u8 keyLock;                 // 0x46
    u8 bluetooth;               // 0x47
    u8 powerSave;               // 0x48
    u8 keyTones;                // 0x49
    u8 ste;                     // 0x4a
    u8 rfGain;                  // 0x4b
    u8 sBarStyle;               // 0x4c
    u8 sqNoiseLev;              // 0x4d
    u32 lastFmtFreq;            // 0x4e
    u8 vox;                     // 0x52
    u16 voxTail;                // 0x53
    u8 txTimeout;               // 0x55
    u8 dimmer;                  // 0x56
    u8 dtmfSpeed;               // 0x57
    u8 noiseGate;               // 0x58
    u8 scanUpdate;              // 0x59
    u8 asl;                     // 0x5a
    u8 disableFmt;              // 0x5b
    u16 pin;                    // 0x5c
    u8 pinAction;               // 0x5e
    u8 lcdInverted;             // 0x5f
    u8 afFilters;               // 0x60
    u8 ifFreq;                  // 0x61
    u8 sBarAlwaysOn;            // 0x62
    u8 filler[29];
} settingsBlock;
