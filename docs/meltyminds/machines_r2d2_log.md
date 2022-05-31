---
tags:
    - 3D
    - Marlin
---

# Melty Modifications Log

![R2D2](../media/img/meltyminds/machines/r2d2.jpg)

## Marlin Configuration

> v2.0.9.3, branch `melty_heisenberg_1.0.0`

- [Ultimaker 2 Marlin example](https://github.com/Ultimaker/Ultimaker2Marlin)
- [Marlin Fork](https://github.com/lejbronika/Marlin_MeltyMinds)

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

#define CUSTOM_MACHINE_NAME "Melty"
```

#### @section extruder

```
#define EXTRUDERS 1
#define DEFAULT_NOMINAL_FILAMENT_DIA 1.75
```

#### @section machine

> line 357

#### @section temperature

```
#define TEMP_SENSOR_0 13
#define TEMP_SENSOR_BED 1

#define TEMP_HYSTERESIS              5

#define HEATER_0_MAXTEMP 310

#define HOTEND_OVERSHOOT 10

#define PIDTEMP

	#define DEFAULT_Kp 29.20 // Trianglelab NTC100K B3950
    #define DEFAULT_Ki 5.66
    #define DEFAULT_Kd 37.64
	
//#define PIDTEMPBED
```

#### @section extruder

```
#define EXTRUDE_MINTEMP 170 // set 5 for extruder flow calibration
```

#### @section machine

> line 727

#### @section homing

```
#define USE_XMIN_PLUG
#define USE_YMAX_PLUG
#define USE_ZMAX_PLUG

#define ENDSTOPPULLUPS

#define X_MIN_ENDSTOP_INVERTING true
#define Y_MAX_ENDSTOP_INVERTING true
#define Z_MAX_ENDSTOP_INVERTING true

#define X_DRIVER_TYPE  TMC2209_STANDALONE
#define Y_DRIVER_TYPE  TMC2209_STANDALONE
#define Z_DRIVER_TYPE  TMC2209_STANDALONE
#define E0_DRIVER_TYPE TMC2209_STANDALONE
```

#### @section motion


```
#define DEFAULT_AXIS_STEPS_PER_UNIT   { 80, 80, 400, 400 }
#define DEFAULT_MAX_FEEDRATE          { 300, 300, 5, 25 }
#define DEFAULT_MAX_ACCELERATION      { 3000, 3000, 250, 10000 }

#define DEFAULT_ACCELERATION          3000    // X, Y, Z and E acceleration for printing moves
#define DEFAULT_RETRACT_ACCELERATION  3000    // E acceleration for retracts
#define DEFAULT_TRAVEL_ACCELERATION   3000    // X, Y, Z acceleration for travel (non printing) moves

//#define CLASSIC_JERK
#define DEFAULT_EJERK    5.0  // May be used by Linear Advance

#if DISABLED(CLASSIC_JERK)
  #define JUNCTION_DEVIATION_MM 0.022 // (mm) Distance from real junction edge
  #define JD_HANDLE_SMALL_SEGMENTS    // Use curvature estimation instead of just the junction angle
                                      // for small segments (< 1mm) with large junction angles (> 135°).
#endif
```

??? Info
	- `DEFAULT_ACCELERATION`: `P`
    - `DEFAULT_RETRACT_ACCELERATION`: `R`
    - `DEFAULT_TRAVEL_ACCELERATION`: `T`

#### @section probes

> line 990

#### @section extruder

> line 1284

#### @section machine

```
#define INVERT_X_DIR true
#define INVERT_Y_DIR false
#define INVERT_Z_DIR true
```

#### @section extruder

```
#define INVERT_E0_DIR true
```

#### @section homing

```
#define X_HOME_DIR -1
#define Y_HOME_DIR 1
#define Z_HOME_DIR 1
```

#### @section machine

```
#define X_BED_SIZE 195
#define Y_BED_SIZE 195

#define X_MIN_POS 0
#define Y_MIN_POS 0
#define Z_MIN_POS 0
#define X_MAX_POS X_BED_SIZE
#define Y_MAX_POS Y_BED_SIZE
#define Z_MAX_POS 193
```

#### @section calibrate

```
#define MESH_BED_LEVELING

//#define MANUAL_PROBE_START_Z 0.1

#define GRID_MAX_POINTS_X 4
#define GRID_MAX_POINTS_Y GRID_MAX_POINTS_X

#define LCD_BED_LEVELING
#define LEVEL_BED_CORNERS
``` 

#### @section homing

> line 1687

#### @section calibrate

> line 1723

#### @section extras

```
#define EEPROM_SETTINGS
```

#### @section temperature

> line 1823

#### @section lcd

```
#define LCD_LANGUAGE en

#define DISPLAY_CHARSET_HD44780 CYRILLIC

#define SDSUPPORT
```

LCD Model-specific:

```
#define MINIPANEL
#define MKS_MINI_12864
```

#### @section extras

> line 2738

### Configuration_adv.h

- `E0_AUTO_FAN_PIN 10`
- `QUICK_HOME` 
- `BABYSTEPPING`
- `LIN_ADVANCE`
    + `LIN_ADVANCE_K 0`
    + `LA_DEBUG`
- `NO_VOLUMETRICS`

### Version.h

### _Bootscreen.h

## 3d-printed parts

### Installed

### Not tested