# Общие заметки по сборке, прошивке и калибровке FDM-принтеров

## Сборка

- [Matterhackers - Hotend assembling guide](https://www.matterhackers.com/articles/how-to-assemble-an-e3d-v6-all-metal-hotend)

## Прошивка

### Machine
- [MKS GEN 1.4 pins](https://raw.githubusercontent.com/makerbase-mks/MKS-GEN/master/hardware/MKS%20GEN%20V1.4_004/MKS%20GEN%20V1.4_004%20PIN.pdf)
- [MKS GEN 1.4 GitHub repo](https://github.com/makerbase-mks/MKS-GEN)

### Temperature
- [Генератор таблицы термитсора для Marlin](https://www.thingiverse.com/thing:103668/files)
- `TEMP_SENSOR_x`:
  - 13: TriangleLab NTC100K B3950
  - 61: Zona3D
  - 1: heatbed

#### Homing
- Stepper Drivers:
    + `A4988`
    + `DRV8825`
    + `TMC2209` - UART connection
    + `TMC2209_STANDALONE` - simple connection
    + [Расчет опорного напряжения для драйверов шаговых двигателей - форум 3deshnik](https://3deshnik.ru/forum/viewtopic.php?f=5&t=78)

#### Motion
- [Расчет Junction Deviation для Marlin](https://blog.kyneticcnc.com/2018/10/computing-junction-deviation-for-marlin.html)
- [Калькулятор расчета шаг/мм для шаговых двигателей](https://blog.prusaprinters.org/calculator_3416/#stepspermmbelt)

### Настройка перемещения по осям

#### Оси X и Y:

- Шаговый двигатель: 200 шагов на оборот, 16 микро-шагов на шаг (ПЕРЕМЫЧКИ)
- Приводной ремень GT2: шаг 2мм
- Шкив: 20 зубьев

Сколько шагов должен совершить двигатель, чтобы по оси X и Y было совершено
перемещение ровно в 1 мм:

(200*16)/(2*20) = 80

#### Ось Z

- Мотор с интегрированным трапецеидальным винтом:
- Диаметр 8 мм
- Шаг 2 мм
- Заходность 4

Количество шагов: (200*16)/(2*4) = 400

### Thermal Protection

В некоторых случаях необходимо увеличить периоды ожидания механизмов термальной
защиты.

```
#if ENABLED(THERMAL_PROTECTION_HOTENDS)
  #define THERMAL_PROTECTION_PERIOD 80        // Seconds
  #define THERMAL_PROTECTION_HYSTERESIS 8     // Degrees Celsius

#define WATCH_TEMP_PERIOD 40                // Seconds
#define WATCH_TEMP_INCREASE 4               // Degrees Celsius

#if ENABLED(THERMAL_PROTECTION_BED)
  #define THERMAL_PROTECTION_BED_PERIOD 40    // Seconds
  #define THERMAL_PROTECTION_BED_HYSTERESIS 4 // Degrees Celsius

  /**
   * As described above, except for the bed (M140/M190/M303).
   */
  #define WATCH_BED_TEMP_PERIOD 120                // Seconds
  #define WATCH_BED_TEMP_INCREASE 2               // Degrees Celsius
#endif
```

### Отключить Buzzer

В файле `pins_MKS_GEN_12.h` изменить следующую константу:

```
#define BEEPER_PIN       -1
```

## Калибровка

- [Repetier-Host BL-Touch calibration manual](https://www.repetier.com/documentation/repetier-firmware/z-probing/)

### Калибровка экструдера

[Клибровка ширны потока](https://corexy3d.blogspot.com/2017/04/extrusion-width-calibration-using.html)

**Разрешение экструдера** - отношение колчества микрошагов шагового двигателя экструдера к величине перемещения филамента. --> характеристкиа принтера

**Поток** - множитель для величины перемещения филамента в движениях печати. 

??? Info
    - Дробление шага мотора эструдера
    - Диаметр подающей шестерни
    - Разрешение мотора
    - Предаточное отношение редуктора (если есть)

1. задать температуру хотэнда выше минимальной: `M104 S200`
2. определить положение экструдера: `M83`
3. выдавить 100мм пластика на небольшой скорости: `G1 E100 F600`
4. узнать текущее значение шаг/мм: `M503`
5. задать новое значение шаг/мм: `M92 EXXX` 
6. сохранить новое значение шаг/мм через EEPROM: `M500`