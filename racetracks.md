---
title: Racetracks
layout: home
---

# Racetracks

```c
enum RacetrackID {
    BiancaCity1     = 0x0C,
    BiancaCity2     = 0x07,
    BiancaCity3     = 0x14,
    StarkFarm1      = 0x09,
    StarkFarm2      = 0x0F,
    StarkFarm3      = 0x13,
    EmpyreanColony1 = 0x0D,
    EmpyreanColony2 = 0x0E,
    CloudCarpet1    = 0x04,
    CloudCarpet2    = 0x05,
    TenthZoneEast1  = 0x01,
    TenthZoneEast2  = 0x11,
    BeaconPort      = 0x02,
    Synobazz1       = 0x0A,
    SynobazzC       = 0x15,
    AncientMesa1    = 0x10,
    AncientMesa2    = 0x06,
    CraterLand1     = 0x0B,
    CraterLand2     = 0x08,
    FireField1      = 0x03,
    FireField2      = 0x12,
    Silence         = 0x16
};
```


## Racetrack metadata (`0x082C2F1C`)

An array of 22 racetrack data structs. It includes the 20 regular GP tracks, the Synobazz championship track and
the Silence Single-PAK track. This array is NOT ordered following GP order, but in `RacetrackID` order.

```c
typedef struct {
    u16 checkpointWidthScaleFactors[4];
    Checkpoint* checkpointData;
    u8* checkpointIndices;
    u8* checkpointLookupMatrix;
    
    u8* minimap;
    
    u8 layoutIdx;
    
    s8 minimapXCoordOffset;
    u8 audiovisualStyleIdx;
    u8 _padding[1];
} RacetrackMetadata;
```

| Offset | Type   | Name                        | Status        | Description                                                                                                                              |
|--------|--------|-----------------------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------|
| +0x00  | u16[4] | checkpointWidthScaleFactors | Confirmed     | Scaling factor that applies to checkpoint of width 0 to 3 respectively. It affects how close to the checkpoint's center the AI ships go. |
| +0x08  | ptr    | checkpointData              | Confirmed     | Pointer to the racetrack's list of checkpoints.                                                                                          |
| +0x0C  | ptr    | checkpointIndices           | Confirmed     | Pointer to the racetrack's list of AI path indices.                                                                                      |
| +0x10  | ptr    | checkpointLookupMatrix      | Needs testing | (Maybe) a lookup table that maps racetrack regions to a reference checkpoint in it.                                                      |
| +0x14  | ptr    | minimap                     | Confirmed     | Pointer to the racetrack's 4bpp minimap.                                                                                                 |
| +0x18  | u8     | layoutIdx                   | Confirmed     | The position in the layout pointer table (`0x08360AAC`) of the racetrack's layout RLE data.                                              |
| +0x19  | s8     | minimapXCoordOffset         | Confirmed     | The X coordinate offset of the minimap's position. It's used to correct dead space on the left of some minimaps.                         |
| +0x1A  | u8     | audiovisualStyleIdx         | Confirmed     | The position in the audiovisual style array (`0x082B4F10`) of the racetrack's audiovisual style data.                                    |
| +0x1B  | u8     | _padding                    | Confirmed     | --                                                                                                                                       |

- **TODO**: Figure out where the pointer to the track name image is stored


## Audiovisual styles (`0x082B4F10`)

An array of 22 audiovisual styles to be used by the racetracks. Some of them are used various times, some of them
are unused.

There's an array at address `0x0804A170` that maps `RacetrackID` to their `AudiovisualStyle` index in the array
at `0x082B4F10`.

```c
typedef struct {
    char[32] areaName;
    
    u8* ptrOstData;
    
    u8* ptrRacetrackTileset;
    u8* ptrRacetrackPalette;
    
    u8* ptrBackgroundTileset;
    u8* ptrBackgroundTilemap1;
    u8* ptrBackgroundTilemap2;
    u8* ptrBackgroundPalette;
    
    u8* ptrGroundTileset;
    u8* ptrGroundPalette;
    u8* ptrGroundUncompressedTilemap;
    s8 idxGroundRLETilemap;

    u8 racetrackHeight;
    u8 _padding[2];
} AudiovisualStyle;
```

| Offset | Type     | Name                         | Status        | Description                                                                                                                                                                                                                                                                        |
|--------|----------|------------------------------|---------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| +0x00  | char[32] | areaName                     | Confirmed     | Area name for the style (e.g. "ANCIENT MESA").                                                                                                                                                                                                                                     |
| +0x20  | ptr      | ptrOstData                   | Confirmed     | Pointer to the music soundtrack of the style.                                                                                                                                                                                                                                      |
| +0x24  | ptr      | ptrRacetrackTileset          | Confirmed     | Pointer to the tileset of the racetrack.                                                                                                                                                                                                                                           |
| +0x28  | ptr      | ptrRacetrackPalette          | Confirmed     | Pointer to the palette for the racetrack.                                                                                                                                                                                                                                          |
| +0x2C  | ptr      | ptrBackgroundTileset         | Confirmed     | Pointer to the tileset of the racetrack's background.                                                                                                                                                                                                                              |
| +0x30  | ptr      | ptrBackgroundTilemap1        | Confirmed     | Pointer to the tilemap for layer 1 of the racetrack's background.                                                                                                                                                                                                                  |
| +0x34  | ptr      | ptrBackgroundTilemap2        | Confirmed     | Pointer to the tilemap for layer 2 of the racetrack's background.                                                                                                                                                                                                                  |
| +0x38  | ptr      | ptrBackgroundPalette         | Confirmed     | Pointer to the palette of the racetrack's background-                                                                                                                                                                                                                              |
| +0x3C  | ptr      | ptrGroundTileset             | Confirmed     | Pointer to the tileset for the ground below the racetrack.                                                                                                                                                                                                                         |
| +0x40  | ptr      | ptrGroundPalette             | Confirmed     | Pointer to the palette for the ground below the racetrack.                                                                                                                                                                                                                         |
| +0x44  | ptr      | ptrGroundUncompressedTilemap | Needs testing | Pointer to the tilemap for the ground below the racetrack. Is `nullptr` if `idxGroundRLETilemap != -1`.                                                                                                                                                                            |
| +0x48  | s8       | idxGroundRLETilemap          | Needs testing | Position in the array of RLE-compressed ground tilemaps (`0x083BB20c`) of the racetrack's one. Is `-1` if `ptrGroundUncompressedTilemap != nullptr`.                                                                                                                               |
| +0x49  | u8       | racetrackHeight              | Confirmed     | Height of the racetrack over the ground below. Can take values 0, 1, 2 and 3. When the ship is below a threshold determined by a table indexed by this value, the machine explodes and there is a Game Over. Value 3 has a special fall effect (Cloud Carpet's no explosion fall). |
| +0x4A  | u8[2]    | _padding                     | Unknown       | Always 0, may be padding.                                                                                                                                                                                                                                                          |

- **TODO** Figure out where the data related to palette animations is stored.

### Tilesets

- Tileset for all racetracks at `0x08360B0C` (256 8x8px 8bpp tiles)
- Tileset for Single-PAK Silence at `0x08364B0C` (256 8x8px 8bpp tiles)
- 22 256-color palettes stored consecutively at `0x08368B0C`. Colors 0-127 are used by the tileset, colors 128-255
  are alternates for in-race palette animation purposes.
- Background tilesets are 4bpp but use 4 16-color palettes each. All tilesets, tilemaps and palettes are stored
  consecutively from `0x083E03D0` to `0x083F14D0`. Tilemaps use BG palettes 8-11. `0x01EF` is the usual value for
  empty tile. Usable tile IDs start at `0x180` (384).


## Layout RLE

The racetrack's layout is compressed using a run-length encoding (RLE). The layouts, uncompressed, consist on a 64x64
tile grid in which each element is the ID of a 4x4 tile-sized chunk. Chunks for all racetracks are defined in the same
tilemap in address `0x08??????`.

```
0x081C872A tenth_zone_east_1
0x081C8E52 beacon_port
0x081C99B0 fire_field_1
0x081Ca3D4 cloud_carpet_1
0x081CABBE cloud_carpet_2
0x081CB436 ancient_mesa_2
0x081CBCBA bianca_city_2
0x081CC3F2 crater_land_2
0x081CCAD2 stark_farm_1
0x081CD296 synobazz_1
0x081CDC12 crater_land_1
0x081CE410 bianca_city_1
0x081CEBEE empyrean_colony_1
0x081CF626 empyrean_colony_2
0x081CFD4E stark_farm_2
0x081D060E ancient_mesa_1
0x081D1100 tenth_zone_east_2
0x081D19FA fire_field_2
0x081D2344 stark_farm_3
0x081D2CD0 bianca_city_3
0x081D3470 synobazz_c
0x081D3C30 silence
```

The commands that form the encoding are:

### 0x00: Raw sequence of literals

The literals in the sequence are appended to the uncompressed layout as is.
```
0x00 0xNN 0xAAAA 0xBBBB ... 
N: amount of numbers in the sequence (u8)
A,B,...: raw literals to append, each one is a u16
```

### 0x10: Sequence of deltas

Each literal in the sequence is added (sum) to the last already uncompressed ID when the literal is read, then appended.

E.g.: if the sequence is `[3, 6, -1]` and the uncompressed layout ends in 82, then IDs 85, 91 and 90 are added.

```
0x10 0xNN 0xAA 0xBB ...
N: amount of numbers in the sequence (u8)
A,B,...: deltas to add to last uncompressed ID, each one is a s8
```

### 0x40: Consecutive indices

A sequence of consecutive increasing IDs is appended after the last already uncompressed ID. It is a special case
of the previous command, in which all deltas would be 1.

E.g.: if the uncompressed layout ends in 42 and n=3, IDs 43, 44 and 45 are added.

```
0x40 0xNN
N: amount of numbers to add minus 1 (e.g. if N = 1, 2 IDs are added), it is a u8
```

### 0x80: Repeat literal

The same ID is appended an amount of times.

```
0x80 0xNN 0xAAAA
A: ID to append, it is a u16
N: amount of times to add A minus 1 (e.g. if N = 1, 2 IDs are added), it is a u8
```

### 0xC0/0xC1: Repeat ID 0

0 is appended an amount of times.

```
0xC0 0xNN
N: amount of times to add 0 minus 1 (e.g. if N = 1, 2 IDs are added), it is a u8
```

`0xC1` is used once in the RLE for Silence, but I don't see any different effect than `0xC0`.


## Checkpoints and indices

Racetrack checkpoints have the following structure:

```
bit_0            bit_1
0AAAAAXXXXXXXXXX 00WWSSYYYYYYYYYY
```

Bit 0 has lower address.

| Letter | Status        | Description                                                                   |
|--------|---------------|-------------------------------------------------------------------------------|
| X      | Confirmed     | X coordinate of the checkpoint's position                                     |
| Y      | Confirmed     | Y coordinate of the checkpoint's position                                     |
| A      | Confirmed     | Angle that the AI has when passing through the checkpoint.                    |
| W      | Needs testing | May be related to how close to the checkpoint's center the AI passes through. |
| S      | Confirmed     | Related to the speed that the AI takes when passing through the checkpoint.   |

Indices refer to a list of pairs of `u8` that reference indices in the racetrack's array of checkpoints. Each racetrack defines two
possible paths for the AI at every point, and indices are stored in pairs, one element for each route.

```
0x08316C20 tenth_zone_east_1 (checkpoints)
0x08316E44 tenth_zone_east_1 (indices)
0x08316F40 beacon_port (checkpoints)
0x083172B8 beacon_port (indices)
0x08317428 fire_field_1 (checkpoints)
0x0831776C fire_field_1 (indices)
0x08317880 cloud_carpet_1 (checkpoints)
0x08317B44 cloud_carpet_1 (indices)
0x08317C7C cloud_carpet_2 (checkpoints)
0x08317F3C cloud_carpet_2 (indices)
0x0831808C ancient_mesa_2 (checkpoints)
0x083182E4 ancient_mesa_2 (indices)
0x083183D8 bianca_city_2 (checkpoints)
0x08318608 bianca_city_2 (indices)
0x08318708 crater_land_2 (checkpoints)
0x083188D4 crater_land_2 (indices)
0x08318998 stark_farm_1 (checkpoints)
0x08318C34 stark_farm_1 (indices)
0x08318D6C synobazz_1 (checkpoints)
0x08319128 synobazz_1 (indices)
0x083192B0 crater_land_1 (checkpoints)
0x0831952C crater_land_1 (indices)
0x0831962C bianca_city_1 (checkpoints)
0x083198A4 bianca_city_1 (indices)
0x083199B8 empyrean_colony_1 (checkpoints)
0x08319CD4 empyrean_colony_1 (indices)
0x08319E10 empyrean_colony_2 (checkpoints)
0x0831A124 empyrean_colony_2 (indices)
0x0831A240 stark_farm_2 (checkpoints)
0x0831A4E4 stark_farm_2 (indices)
0x0831A60C ancient_mesa_1 (checkpoints)
0x0831A928 ancient_mesa_1 (indices)
0x0831AA68 tenth_zone_east_2 (checkpoints)
0x0831AD5C tenth_zone_east_2 (indices)
0x0831AEAC fire_field_2 (checkpoints)
0x0831B16C fire_field_2 (indices)
0x0831B294 stark_farm_3 (checkpoints)
0x0831B5FC stark_farm_3 (indices)
0x0831B728 bianca_city_3 (checkpoints)
0x0831B9F0 bianca_city_3 (indices)
0x0831BB3C synobazz_c (checkpoints)
0x0831BD10 synobazz_c (indices)
0x0831BDFC silence (checkpoints)
0x0831C04C silence (indices)
```


## Minimaps

Each one is a 4bpp 64x64px image. They use 2 colors from a palette (`0x08??????`) shared with more UI elements.

- **TODO**: There should be at least a couple other colors available, find my notes on it and confirm.

```
0x0836C70C tenth_zone_east_1
0x0836CF0C beacon_port
0x0836D70C fire_field_1
0x0836DF0C cloud_carpet_1
0x0836E70C cloud_carpet_2
0x0836EF0C ancient_mesa_2
0x0836F70C bianca_city_2
0x0836FF0C crater_land_2
0x0837070C stark_farm_1
0x08370F0C synobazz_1
0x0837170C crater_land_1
0x08371F0C bianca_city_1
0x0837270C empyrean_colony_1
0x08372F0C empyrean_colony_2
0x0837370C stark_farm_2
0x08373F0C ancient_mesa_1
0x0837470C tenth_zone_east_2
0x08374F0C fire_field_2
0x0837570C stark_farm_3
0x08375F0C bianca_city_3
0x0837670C synobazz_c
0x08376F0C silence
```