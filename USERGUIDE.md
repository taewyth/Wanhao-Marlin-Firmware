# User Guide

## Touchscreen calibration

On first boot, you may have a little ➕ symbol displayed on screen. This is for touchscreen calibration. Touch the symbol, it will move to another location. Repeat until calibration is done.

## Autoleveling

If you have a 3D touch, turn on printer, go to _⚙ > Maintenance > Autoleveling_ and perform autoleveling once. This may take a while. Then, no need to autolevel once per print. You should perform Autoleveling again if moving the printer or not using it for a while.

Once autoleveling is done, home Z axis using home screen controls and go to _⚙ > Configuration > Probe Z Offset_. Adjust Z offset until the head is at desired head (should be lightly scratching a sheet of paper). You should need to adjust Z offset again only if detaching/reattaching the 3D touch probe from the print head.

## Filament load / unload

You can also use the Load/Unload macros in _⚙ > Maintenance_, or adjust heat and operate extruder directly using controls from the home screen.

## Start / End gcode

I'd recommend using the following start/end gcodes in your slicer. Feel free to customize them.

### Start Gcode

```
; === Start Gcode ===
M107 ; stop fan
M104 S200 ; Set nozzle temperature to 200°C and proceed
M190 S60 ; Wait for bed temperature to reach 60°C
M109 S200 ; Wait for nozzle temperature to reach 200°C
G28 ; home all axes
M420 S1 Z10 ; Retrieve previous autoleveling settings
M117 ; Purge extruder
G92 E0 ; Reset extruder
G1 Z1.0 F300 ; Move Z up a little to prevent scratching of surface
G1 X2 Y20 F3000.0 ; Move X/Y to start-line position
G1 Z0.3 F300.0 ; Move Z down to start-line height
G1 X2 Y200.0 Z0.3 F1500.0 E15 ; Draw 1st line
G1 X2 Y200.0 Z0.4 F3000.0 ; Move to side a little
G1 X2 Y20 Z0.4 F1500.0 E30 ; Draw 2nd line
G92 E0 ; Reset extruder
G1 Z1.0 F300 ; Move Z up little to prevent scratching of surface
; === End Start Gcode ===
```

Note: `M420 S1 Z10 ; Retrieve previous autoleveling settings` should have no effect on non-3D touch printers, but you can remove it if you do not have 3D touch.

### End Gcode

```
; === End Gcode ===
M104 S0 ; turn off temperature
G28 X0  ; home X axis
M84     ; disable motors
; Play some Beeps at end of print
M300 P1000
G4 P2000
M300 P1000
G4 P2000
M300 P1000
; === End of End Gcode ===
```

Note: This Gcode plays beeps at end of print. Remove the associated instructions to disable.
