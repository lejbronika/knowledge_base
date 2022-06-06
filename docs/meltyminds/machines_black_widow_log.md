---
tags:
    - 3D
    - Marlin
---

# Black Widow Modifications Log

## Marlin Configuration

> meltyminds_0.0.1

- [Tevo oficial image](https://github.com/Homers3D/Tevo-Black-Widow)
- [MeltyMinds Fork 2.0.x](https://github.com/lejbronika/Marlin_TBW)

### Configuration.h

#### @section info

```
#define STRING_CONFIG_H_AUTHOR "lejbron"
#define CUSTOM_VERSION_FILE Version.h
#define SHOW_CUSTOM_BOOTSCREEN
```

#### @section machine

```
#define BAUDRATE 250000

	#define MOTHERBOARD BOARD_MKS_GEN_13

#define CUSTOM_MACHINE_NAME "Black Widow"
```

#### @section extruder

```
#define EXTRUDERS 1
#define DEFAULT_NOMINAL_FILAMENT_DIA 1.75
```

#### @section machine

> line 399

#### @section temperature

> line 446

```
#define TEMP_SENSOR_0 61
#define TEMP_SENSOR_BED 11

#define TEMP_HYSTERESIS              5

#define HEATER_0_MAXTEMP 310

#define HOTEND_OVERSHOOT 10

#define PIDTEMP
	
//#define PIDTEMPBED
```

#### @section extruder

> line 800

```
#define EXTRUDE_MINTEMP 170
```

#### @section machine

> line 845

#### @section homing

> line 872

```
#define USE_XMIN_PLUG
#define USE_YMIN_PLUG
#define USE_ZMIN_PLUG

#define ENDSTOPPULLUPS

#define X_MIN_ENDSTOP_INVERTING true
#define Y_MIN_ENDSTOP_INVERTING true

#define X_DRIVER_TYPE  DRV8825
#define Y_DRIVER_TYPE  DRV8825
#define Z_DRIVER_TYPE  DRV8825
#define E0_DRIVER_TYPE DRV8825
```

#### @section motion

> line 991

```
#define DEFAULT_AXIS_STEPS_PER_UNIT   { 160, 160, 3200, 610 }
#define DEFAULT_MAX_FEEDRATE          { 300, 300, 5, 25 }
#define DEFAULT_MAX_ACCELERATION      { 1000, 1000, 100, 10000 }

#define DEFAULT_ACCELERATION          1000    // X, Y, Z and E acceleration for printing moves
#define DEFAULT_RETRACT_ACCELERATION  3000    // E acceleration for retracts
#define DEFAULT_TRAVEL_ACCELERATION   1000    // X, Y, Z acceleration for travel (non printing) moves

//#define CLASSIC_JERK
#define DEFAULT_EJERK    5.0  // May be used by Linear Advance

#if DISABLED(CLASSIC_JERK)
  #define JUNCTION_DEVIATION_MM 0.013 // (mm) Distance from real junction edge
  #define JD_HANDLE_SMALL_SEGMENTS    // Use curvature estimation instead of just the junction angle
                                      // for small segments (< 1mm) with large junction angles (> 135°).
#endif
```

??? Info
	- `DEFAULT_ACCELERATION`: `P`
    - `DEFAULT_RETRACT_ACCELERATION`: `R`
    - `DEFAULT_TRAVEL_ACCELERATION`: `T`

#### @section probes

> line 1108

#### @section extruder

> line 1428

#### @section machine

> line 1433

```
#define INVERT_X_DIR false
#define INVERT_Y_DIR true
#define INVERT_Z_DIR true
```

#### @section extruder

> line 1446

```
#define INVERT_E0_DIR false
```

#### @section homing

> line 1458

```
#define X_HOME_DIR -1
#define Y_HOME_DIR -1
#define Z_HOME_DIR -1
```

#### @section machine

> line 1487

```
#define X_BED_SIZE 348
#define Y_BED_SIZE 220

#define X_MIN_POS 0
#define Y_MIN_POS 0
#define Z_MIN_POS 0
#define X_MAX_POS X_BED_SIZE
#define Y_MAX_POS Y_BED_SIZE
#define Z_MAX_POS 280
```

#### @section calibrate

> line 1632

```
#define MESH_BED_LEVELING

//#define MANUAL_PROBE_START_Z 0.1

#define GRID_MAX_POINTS_X 4
#define GRID_MAX_POINTS_Y GRID_MAX_POINTS_X

#define LCD_BED_LEVELING
#define LEVEL_BED_CORNERS
``` 

#### @section homing

> line 1849

#### @section calibrate

> line 1886

#### @section extras

> line 1947

```
#define EEPROM_SETTINGS

#define EEPROM_INIT_NOW
```

#### @section temperature

> line 1987

#### @section lcd

> line 2180

```
#define LCD_LANGUAGE en

#define DISPLAY_CHARSET_HD44780 CYRILLIC

#define SDSUPPORT
```

LCD Model-specific:

```
#define MKS_MINI_12864_V3
#define NEOPIXEL_LED

    #define NEOPIXEL_TYPE          NEO_RGB
```

#### @section extras

> line 2954

### Configuration_adv.h

- `QUICK_HOME` 
- `BABYSTEPPING`
- `NO_VOLUMETRICS`


### BL Touch Config 

[Configure Bl-Touch in Marlin 2.0.x](https://3dwork.io/en/configure-bltouch-in-marlin/)
[K3D yiutube guide](https://www.youtube.com/watch?v=oJgKQKbN8nE)
 
```
#define Z_MIN_ENDSTOP_INVERTING false

#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN

#define BLTOUCH

#define NOZZLE_TO_PROBE_OFFSET { -11, -37, -2 }

#define MULTIPLE_PROBING 2

#define Z_MIN_PROBE_REPEATABILITY_TEST

#define AUTO_BED_LEVELING_BILINEAR
#define RESTORE_LEVELING_AFTER_G28

#define G26_MESH_VALIDATION

#define GRID_MAX_POINTS_X 4
#define GRID_MAX_POINTS_Y GRID_MAX_POINTS_X

#define Z_SAFE_HOMING
 
#if ENABLED(Z_SAFE_HOMING)
#define Z_SAFE_HOMING_X_POINT ((X_BED_SIZE) / 2) // X point for Z homing when homing all axes (G28).
#define Z_SAFE_HOMING_Y_POINT ((Y_BED_SIZE) / 2) // Y point for Z homing when homing all axes (G28).
#endif
```
- Обнулить Z-offset
- выполнить autohome
- отключить концевики
- выставить Z-0ffset
- сохранить значения
- включить концевики

G-Code:

```
G28
G1 F500 Z0
M211 S0 ; disable endstops
G91 ; akternative coordinates
G1 Zx ...
M114 ; get current position
M211 S1 ; enable endstops
G90 ; absolute coordinates
```

#### Offset Calibration

- Connect to PC
- !preheat!
- `G28`
- `G1 F500 Z0`
- `Configuration` -> `Probe Z Offset`
- Calibrate with sheet of paper
- `Configuration` -> `Store Settings`
- Check with sheet of paper: `G28 G1 F500 Z0`

z_offset = calculated_offsed + paper_thicknes (~0.1mm)

TBW_z_offset = -2,375 + 0,35 = -2,275

## 3d-printed parts

### Installed

- [Big Upgrade](https://www.thingiverse.com/thing:4278682) (partly installed)
- [Z-Axis G2T Belt Relocation](https://www.thingiverse.com/thing:2168986)
- [BL-Touch mount](https://www.thingiverse.com/thing:2189890)

### Not tested

- [Nozzle fan duct](https://www.thingiverse.com/thing:2175680)

#### Mechanics

- [!5 Degree extruder mount](https://www.thingiverse.com/thing:1789293)
- [HE cable management](https://www.thingiverse.com/thing:1918783)
- [Corner Plates v1](https://www.thingiverse.com/thing:1858082)
- [Corner Plates v2](https://www.thingiverse.com/thing:2016601)
- [Corner Plates v3 (thick)](https://www.thingiverse.com/thing:2063853)
- [Heated Bed Cable Chain Lock](https://www.thingiverse.com/thing:2302989)
- [X Belt Tensioner v1](https://www.thingiverse.com/thing:3309080)
- [X Belt Tensioner v2](https://www.thingiverse.com/thing:4648374)

##### MGN12 upgrades

- [X-carriage MGN12 conversion](https://www.thingiverse.com/thing:2566111)
- [Y-Carriage MGN12 Conversion](https://www.thingiverse.com/thing:2569059)
- [MGN12 Guide Tool for 2040 Extrusion](https://www.thingiverse.com/thing:2566111)

#### Electronics Case

- [Original case](https://www.thingiverse.com/thing:2125506)
- [Electronics housing upgrade](https://www.thingiverse.com/thing:2346801)
- [Rear opening cap](https://www.thingiverse.com/thing:2672795)

#### Rebuild projects

- [MGN12 XY upgrade](https://www.thingiverse.com/thing:2984957)

#### Other

- [RPi Mount](https://www.thingiverse.com/thing:2620241)
