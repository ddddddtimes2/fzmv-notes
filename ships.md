---
title: Ships
layout: home
---

# Ships

```c
typedef enum {
    FireBall        = 0x00,
    SlyJoker        = 0x01,
    WindWalker      = 0x02,
    JBCrystal       = 0x03,
    HotViolet       = 0x04,
    FalconMKII      = 0x05,
    SilverThunder   = 0x06,
    TheStingray     = 0x07,
    FightingComet   = 0x08,
    JetVermillion   = 0x09,
    NPCBlue         = 0x0A,
    NPCGreen        = 0x0B,
    NPCYellow       = 0x0C,
    Unknown0x0D     = 0x0D,
    ShipMultiplayer = 0x0E
} ShipID;
```


## Stat blocks (`0x082C1CC0`)

The game has a total of 25 stat blocks at address `0x082C1CC0`. Positions 0-9 correspond to the stats of the 10
playable ships, ordered by `ShipID`. Positions 10-19 are an unused clone of the previous stat blocks. Positions 20-24 are unused but
different stat blocks.

```c
typedef struct {
    s8  acceleration[4];
    s16 accThresholds[3];

    s16 normalHysteresisMax;
    s16 normalSpeedRetention;
    s16 normalHysteresisMin;

    u16 boostAcceleration;
    s16 boostHysteresisMax;
    u16 boostSpeedRetention;
    s16 boostHysteresisMin;
    u16 boostTime;

    u16 jump;

    u16 _BRK_SP;
    u8  coastingDeceleration;
    u8  brakingDeceleration;

    u16 grip;
    u16 turning;
    u16 blastTurning;
    u8  slipLimitAngle;
    s8  slipDeceleration;
    s16 slipCoyoteTime;

    u16 strafingSpeed;

    u8  weight_UNUSED;
    u8  collisionRecoveryTime;
    u8  health;
    u8  _POWER;
} MachineStatblock;
```

| Offset | Type   | Name                   | Status         | Unit                                   | Description                                                                                                        |
|--------|--------|------------------------|----------------|----------------------------------------|--------------------------------------------------------------------------------------------------------------------|
| +0x00  | s8[4]  | acceleration           | Confirmed      | 1/4ths of kmh/frame                    | Acceleration of the ship for each "gear".                                                                          |
| +0x04  | s16[3] | accelerationThresholds | Confirmed      | 1/4ths of km/h                         | Speed thresholds at which the ship changes gears (acc. value).                                                     |
| +0x0A  | s16    | normalHysteresisMax    | Confirmed      | 1/4ths of km/h                         | Maximum value for the ship's normal speed.                                                                         |
| +0x0C  | s16    | normalDeceleration     | Confirmed      | 1/4ths of kmh/frame                    | Deceleration rate between max. and min. speed thresholds when throttling normally.                                 |
| +0x0E  | s16    | normalHysteresisMin    | Confirmed      | 1/4ths of km/h                         | Minimum value for the ship's normal speed.                                                                         |
| +0x10  | u16    | boostAcceleration      | Confirmed      | 1/4ths of kmh/frame                    | Acceleration of the ship while boosting.                                                                           |
| +0x12  | s16    | boostHysteresisMax     | Confirmed      | 1/4ths of km/h                         | Maximum value for the ship's boost speed.                                                                          |
| +0x14  | u16    | boostDeceleration      | Confirmed      | 1/4ths of kmh/frame                    | Deceleration rate between max. and min. speed thresholds when boosting.                                            |
| +0x16  | s16    | boostHysteresisMin     | Confirmed      | 1/4ths of km/h                         | Minimum value for the ship's boost speed.                                                                          |
| +0x18  | u16    | boostTime              | Confirmed      | frames                                 | Duration of the ship's boost.                                                                                      |
| +0x1A  | u16    | jump                   | Confirmed      |                                        | Maximum height that a ship reaches when jumping (they increase and decrease their height at a constant rate).      |
| +0x1C  | u16    | _BRK_SP                | Unknown        |                                        | ???                                                                                                                |
| +0x1E  | u8     | coastingDeceleration   | Confirmed      | 1/4ths of kmh/frame                    | Deceleration of the ship while coasting (not throttling nor braking).                                              |
| +0x1F  | u8     | brakingDeceleration    | Confirmed      | 1/4ths of kmh/frame                    | Deceleration of the ship while braking.                                                                            |
| +0x20  | u16    | grip                   | Confirmed      | 0x0000 = 0º, ..., 0x10000 = 360º       | Rate per frame at which the ship's facing direction changes when turning.                                          |
| +0x22  | u16    | turning                | Confirmed      | 0x0000 = 0º, ..., 0x10000 = 360º       | Rate per frame at which the ship's momentum direction changes when turning.                                        |
| +0x24  | u16    | blastTurning           | Confirmed      | 0x0000 = 0º, ..., 0x10000 = 360º       | Rate per frame at which the ship's momentum direction changes when blast turning.                                  |
| +0x26  | u8     | slipLimitAngle         | Confirmed      | 0x00 = 0º, ..., 0x100 = 360º           | Maximum allowed difference between the ship's facing and momentum directions before losing control. Capped at 90º. |
| +0x27  | s8     | slipDeceleration       | Confirmed      | 1/4ths of kmh/frame                    | Deceleration when the ship has slipped (lost control because of angle difference).                                 |
| +0x28  | s16    | slipCoyoteTime         | Testing needed | frames                                 | (Might be) the time that the ship is allowed to be over its limit angle before actually losing control.            |
| +0x2A  | u16    | strafingSpeed          | Confirmed      | 1/4ths of km/h                         | Lateral speed applied when strafing. Totally independent from throttling/boost speed.                              |
| +0x2C  | u8     | _WEIGHT                | Unknown        |                                        | May be unused.                                                                                                     |
| +0x2D  | s8     | collisionRecoveryTime  | Confirmed      | frames                                 | Time that the player loses control of the ship after colliding with another ship.                                  |
| +0x2E  | u8     | health                 | Confirmed      | 0x00 = 0, 0x01 = 64, ..., 0xFF = 16320 | Health points of the ship (POW healthbar).                                                                         |
| +0x2F  | u8     | _POWER                 | Unknown        |                                        | ???                                                                                                                |

- `_BRK_SP`, `_WEIGHT` and `_POWER` are named here after their name in the game's debug menus.
- **TODO**: Find out which stat defines the time needed to get a rocket start.
- **TODO**: Find out whether `collisionRecoveryTime` is also used when colliding with a bomb.


## In-race sprites

```c
typedef struct {
    u8* rleFrameEncoding;
    u8* frameImage;
} ShipRacePlayerSpriteMetadata;

typedef struct {
    u16 vramOffset_pow2;
    u16 frameSize_pow2;
    u8* spritesheet;
} ShipRaceNPCSpriteMetadata;

typedef struct {
    ShipRacePlayerSpriteMetadata[11]* playerSpriteData;
    ShipRaceNPCSpriteMetadata* npcSpriteData;
    s8[4][2] playerSpriteFlameCoords;
    s8 playerSpriteHardTurnSparkCoordOffsets[2];
    u8 playerSpriteAmountOfFlames;
    s8 spriteVerticalOffset;
} ShipRaceSpritesMetadata;
```

There are two types of ship sprites used in a race: "player" sprites (sprite used when the player controls the machine)
and "NPC" sprites (sprite used for the CPU machines, also used in the ship selection menu for Training mode). Sprites
of both kinds are stored consecutively for each machine. Backmarker/NPC ships only have an NPC sprite.

- **NPC sprites**: they are stored as a spritesheet of 32 frames, 32x32 pixels each, depicting the machine at
  different orientations.
- **Player sprites**: these are formed by 11 frames. 0-8 are for the ship at various degrees of turning, 9-10 are
  used when the ship jumps. They are stored as 8x8px chunks, skipping empty spaces.

### Sprites metadata (`0x082C2170`)

A 15-element array of `ShipRaceSpritesMetadata` is stored at `0x082C2170`. Indices `0x00-0x0C` are ordered by `ShipID`.

| Offset | Type                              | Name                                  | Status    | Description                                                                                                                                                                |
|--------|-----------------------------------|---------------------------------------|-----------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| +0x00  | ShipRacePlayerSpriteMetadata[11]* | playerSpriteData                      | Confirmed | Pointer to an array of 11 `ShipRacePlayerSpriteMetadata` structs, one for each frame.                                                                                      |
| +0x04  | ShipRaceNPCSpriteMetadata*        | npcSpriteData                         | Confirmed |                                                                                                                                                                            |
| +0x08  | s8[4][2]                          | playerSpriteFlameCoords               | Confirmed | A 4-element array of (x,y) coordinates. Each point is the coordinates of a flame that will be drawn in one of the ship's exhaust tubes. (0,0) is used for unused elements. |
| +0x10  | s8[2]                             | playerSpriteHardTurnSparkCoordOffsets | Confirmed | (x,y) offsets of the spark sprites that appear on the ship's player sprite when making a hard turn.                                                                        |
| +0x12  | u8                                | playerSpriteAmountOfFlames            | Confirmed | Amount of flames to depict in the ship's player sprite. Capped to a maximum of 4.                                                                                          |
| +0x13  | s8                                | spriteVerticalOffset                  | Confirmed | Vertical offset for the sprites. Applies to both player and NPC sprites.                                                                                                   |

- **TODO** The Y coordinate at which the ship's player sprite is drawn may be stored in one of the unknown values here.
- **TODO** I can't remember where the palettes for all of these sprites are stored

### Player sprites

Player sprites are stored in separate frames, in 8x8-pixel chunks. Empty chunks are skipped. A small RLE encodes how
to unpack the chunks of each frame. Each byte in the RLE alternates between representing an amount of empty data or
image chunks to read. E.g.:

```
RLE:
0x00, 0x60, 0x00, 0x20, 0x20, 0x20

Means:
Output 0 image chunks
Output 6 empty chunks
(line break)
Output 0 image chunks
Output 2 empty chunks
Output 2 image chunks (reads 2 chunks and advances data-reading pointer)
Output 2 empty chunks
(line break)
```

Each byte actually represents an amount of 32-bit integers to read. (0x10 equals 16 u32 values to read equals an 8x8px
8bpp chunk). The RLE doesn't mix the encodings of two different image rows, and the values for the RLE of a row must
add up to `0x60`.

### NPC sprites

| Offset | Type | Name            | Status        | Description                                                                                                                                                                                          |
|--------|------|-----------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| +0x00  | u16  | vramOffset_pow2 | Needs testing | Appears to be an offset to the VRAM position the data is loaded at. Used like `vramDest = 0x06100000 + (1 << vramOffset_pow2)`. Capped at `0x06100000` (start of OBJ VRAM). Probably needs to be 6.  |
| +0x02  | u16  | frameSize_pow2  | Needs testing | Appears to determine the amount of bytes to read from the spritesheet to display a certain frame. Also determines the element size. Used like `size = 1 << frameSize_pow2`. Probably needs to be 10. |
| +0x04  | u8*  | spritesheet     | Confirmed     | Pointer to the ship's spritesheet image.                                                                                                                                                             |
