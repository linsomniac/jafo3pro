# Jafo 3 Pro

An Ender 3 Pro upgraded with:

- SKR Mini E3 V2.0
- Creality Sprite Hotend/Extruder
- CR-Touch
- X and Y steppers upgraded with "Usongshine Nema 17 Stepper Motor 42BYGH 1.8 Degree 38MM 1.5A 42 Motor (17HS4401S)"

This repo includes a Klipper config for this printer.

## Why

It was mostly about wanting to get auto bed leveling on the Ender 3 Pro.

My steppers had started losing steps, and I couldn't find any issues in
the X/Y travel or belts, and the steppers were already at a reasonable
voltage, and were running warm, so I replaced them and the level shifting
stopped.

## Start/End G-Code

Start G-Code needs to be:

```
; Ender 3 Custom Start G-code
G92 E0 ; Reset Extruder
G28 ; Home all axes
BED_MESH_CALIBRATE
BED_MESH_PROFILE SAVE=p1
G1 X0 Y0 Z5 F4000
;G29 ; Auto Bed Level with BL-Touch
;G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
;G1 X0.1 Y20 Z0.3 F5000.0 ; Move to start position
;G1 X0.1 Y200.0 Z0.3 F1500.0 E15 ; Draw the first line
;G1 X0.4 Y200.0 Z0.3 F5000.0 ; Move to side a little
;G1 X0.4 Y20 Z0.3 F1500.0 E30 ; Draw the second line
G92 E0 ; Reset Extruder
G1 Z2.0 F3000 ; Move Z Axis up little to prevent scratching of Heat Bed
G1 X5 Y20 Z0.3 F5000.0 ; Move over to prevent blob squish
```

And End G-Code:

```
G91 ;Relative positioning
G1 E-2 F2700 ;Retract a bit
G1 E-2 Z0.2 F2400 ;Retract and raise Z
G1 X5 Y5 F3000 ;Wipe out
G1 Z10 ;Raise Z more
G90 ;Absolute positioning
G1 X0 Y{machine_depth} ;Present print
M106 S0 ;Turn-off fan
M104 S0 ;Turn-off hotend
M140 S0 ;Turn-off bed
M84 X Y E ;Disable all steppers but Z
```
