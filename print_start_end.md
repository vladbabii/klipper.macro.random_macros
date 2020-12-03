```
[gcode_macro END_PRINT]
gcode =
    G1 Y190 F1500; bring Y up front 
    G10 ; set tool offset?  or retract?
    G91; Relative Positioning
    G1 Z+20; Move Z up so it doesn't hit anything
    G1 E-10 F300; Retrack-10
    G90; Absolute Positioning
    G1 X10 Y220 F2000; Move to X10, Y220
    M104 S0; Turn off Extrude (set it to 0)
    M140 S0; Turn off Bed (set it to 0)
    M106 S0; turn off cooling fan
    M84; Disable steppers
```
---
