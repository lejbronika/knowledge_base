---
tags:
    - 3D
    - Marlin
---

# Black Widow Modifications Log

## Marlin Configuration

> meltyminds_0.0.1

- [Tevo oficial image](https://github.com/Homers3D/Tevo-Black-Widow)

## Configuration.h

#### @section info
    - STRING_CONFIG_H_AUTHOR "lejbron"
    - SHOW_CUSTOM_BOOTSCREEN

#### @section machine
    - MOTHERBOARD BOARD_MKS_GEN_13
    - BAUDRATE 25000
    - CUSTOM_MACHINE_NAME "Black Widow"
    - Stepper Drivers:
        + A4988
        + DRV8825
        + 'TMC2209' - UART connection
        + 'TMC2209_STANDALONE' - simple connection
        
- [Расчет опорного напряжения для драйверов шаговых двигателей - форум 3deshnik](https://3deshnik.ru/forum/viewtopic.php?f=5&t=78)
- [MKS GEN 1.4 pins](https://raw.githubusercontent.com/makerbase-mks/MKS-GEN/master/hardware/MKS%20GEN%20V1.4_004/MKS%20GEN%20V1.4_004%20PIN.pdf)
- [MKS GEN 1.4 GitHub repo](https://github.com/makerbase-mks/MKS-GEN)

##### @section extruder
    - EXTRUDERS 1
    - DEFAULT_NOMIANL_FILAMENT_DIA 1.75

##### @section temperature
    - TEMP_SENSOR_0 61
    - TEMP_SENSOR_BED 1 
    - EXTRUDE_MINTEMP
    - TEMP_HYSTERESIS 5
    - HEATER_0_MAXTEMP 310
    - HOTEND_OVERSHOOT 10
    - PIDTEMP
    - PIDTEMPBED

- [NTC100K до 350 градусов](https://zona-3d.ru/catalog/elektronika/termodatchiki/termorezistory/datchik_temperatury_ntc100k_v_korpuse_maks_350_s_2_metra)
- [3d-today question](https://3dtoday.ru/questions/ht-ntc100k-v-marlin-2-kakoy-nomer-tip-sootvetstvuet)
- [Генератор таблицы термитсора для Marlin](https://www.thingiverse.com/thing:103668/files)

##### @section homing
    - USE_XMIN_PLUG
    - USE_YMIN_PLUG
    - USE_ZMIN_PLUG
    - ENDSTOPPULLUPS
    - X_MIN_ENDSTOP_INVERTING true 
    - Y_MIN_ENDSTOP_INVERTING true 
    - Z_MIN_ENDSTOP_INVERTING true

#### @section probes
    - Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN
    
#### @section calibrate
    - comment out: MANUAL_PROBE_START_Z
    - GRID_MAX_POINTS_X 4

##### @section motion
    - DEFAULT_AXIS_STEPS_PER_UNIT   {160,160,3200,610}
    - DEFAULT_MAX_FEEDRATE          { 300, 300, 5, 25 }
    - DEFAULT_MAX_ACCELERATION      { 1000, 1000, 100, 10000 }
    
    - P - DEFAULT_ACCELERATION          1000    // X, Y, Z and E acceleration for printing moves 
    - R - DEFAULT_RETRACT_ACCELERATION  3000    // E acceleration for retracts
    - T - DEFAULT_TRAVEL_ACCELERATION   1000    // X, Y, Z acceleration for travel (non printing) moves

    - DEFAULT_XYJERK 20.0    // (mm/sec)
    - DEFAULT_ZJERK 0.4     // (mm/sec)

- [Расчет Junction Deviation для Marlin](https://blog.kyneticcnc.com/2018/10/computing-junction-deviation-for-marlin.html)
- [Калькулятор расчета шаг/мм для шаговых двигателей](https://blog.prusaprinters.org/calculator_3416/#stepspermmbelt)

##### MIX
    - INVERT_X_DIR true
    - INVERT_Y_DIR true
    - INVERT_Z_DIR true
    - INVERT_E0_DIR false
    - X_HOME_DIR -1
    - Y_HOME_DIR -1
    - Z_HOME_DIR -1
    - X_BED_SIZE 380
    - Y_BED_SIZE 250
    - Z_MAX_POS 300
    - comment out: MIN_SOFTWARE_ENDSTOPS

##### @section calibrate:
    - MESH_BED_LEVELING
    - LCD_BED_LEVELING
    - LEVEL_BED_CORNERS

##### @section extras:
    - EEPROM_SETTINGS

##### @section lcd:
    - LCD_LANGUAGE en
    - DISPLAY_CHARSET_HD44780 CYRILLIC
    - SDSUPPORT
    - MKS_MINI_12864_V3MKS_MINI_12864
    - NEOPIXEL_LED

## BL Touch Config 

- [Configure Bl-Touch in Marlin 2.0.x](https://3dwork.io/en/configure-bltouch-in-marlin/)
 
```
#define Z_MIN_ENDSTOP_INVERTING false

#define Z_MIN_PROBE_USES_Z_MIN_ENDSTOP_PIN

#define BLTOUCH

#define NOZZLE_TO_PROBE_OFFSET { 41, 8, 0 }

#define MULTIPLE_PROBING 2

#define Z_MIN_PROBE_REPEATABILITY_TEST

#define X_BED_SIZE 380
#define Y_BED_SIZE 250

#define AUTO_BED_LEVELING_BILINEAR

#define G26_MESH_VALIDATION

#define GRID_MAX_POINTS_X 4
#define GRID_MAX_POINTS_Y GRID_MAX_POINTS_X

#define Z_SAFE_HOMING


#define RESTORE_LEVELING_AFTER_G28
```

### Калибровка

https://www.repetier.com/documentation/repetier-firmware/z-probing/

```
M851 Z0
```

## Configuration_adv.h

- `E0_AUTO_FAN_PIN 10`
- `QUICK_HOME` 
- `BABYSTEPPING`
- `LIN_ADVANCE`
    + `LIN_ADVANCE_K 0`
    + `LA_DEBUG`
- `NO_VOLUMETRICS`

## Mechanics

- [10mm y-axis tensioner remix](https://www.thingiverse.com/thing:3255414)
- [Base Stabilizer v1](https://www.thingiverse.com/thing:2119307)
- [HE cable management](https://www.thingiverse.com/thing:1918783)
- [V-Slot Corner Bracket](https://www.thingiverse.com/thing:1764563)
- [Corner Plates v1](https://www.thingiverse.com/thing:1858082)
- [Corner Plates v2](https://www.thingiverse.com/thing:2016601)
- [Center Plate](https://www.thingiverse.com/thing:4302036)
- [MGN12 Guide Tool for 2040 Extrusion](https://www.thingiverse.com/thing:2566111)
- [Corner Plates v3 (thick)](https://www.thingiverse.com/thing:2063853)
- [Heated Bed Cable Chain Lock](https://www.thingiverse.com/thing:2302989)
- [X Belt Tensioner v1](https://www.thingiverse.com/thing:3309080)
- [X Belt Tensioner v2](https://www.thingiverse.com/thing:4648374)
- [Lower Corner Brace 60x60x80](https://www.thingiverse.com/thing:4608625)
- [Y-Carriage MGN12 Conversion](https://www.thingiverse.com/thing:2569059)
- [Z-Axis G2T Belt Relocation](https://www.thingiverse.com/thing:2168986)
- [Extruder spacer](https://www.thingiverse.com/thing:2188117)
- [Y-axis tensioner](https://www.thingiverse.com/thing:2265301)

## Electronics Case

- [Electronics housing upgrade](https://www.thingiverse.com/thing:2346801)
- [Rear opening cap](https://www.thingiverse.com/thing:2672795)

## Tool Head Upgrades

- [Chelicerae 537](https://www.thingiverse.com/thing:3468874)
- [Chimera Fan Shroud](https://www.thingiverse.com/thing:4395373)
