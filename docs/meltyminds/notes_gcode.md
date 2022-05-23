# G-code Notes

## Prusa Custom G-code

Tested g-code snippets for [Prusa Slicer](https://www.prusa3d.com/).

### Start G-code

```
G28 ; home all axes
G1 Z70 F5000 ; lift nozzle
```

### End G-code

```
G1 E-4 F1800 ; retract 4 mm
M104 S0 ; turn off temperature
M140 S0; Set bed to 0C (off)
G28 X0  ; home X axis
M84     ; disable motors
```

## Manual Control

- `M119` - check endstops state
- `G1 E30 F1800` - extrude 30mm on 60 mm/sec speed