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

```
gcode: 
    M221 S96;
    SET_RETRACTION RETRACT_LENGTH=0.7 RETRACT_SPEED=25
    SET_PRESSURE_ADVANCE ADVANCE=0.05322
    INIT_PRINT BED_TEMP=60 EXTRUDER_TEMP=225
```

---

```
[gcode_macro END_PRINT]
gcode:
    G91
    #G1 E-3 F6000   ; Retract 
    G10
    G1 Z+10 F6000 ; Move Up
    M140 S0             ; Bed off
    M104 S0             ; Extruder off
    M106 S0             ; Fan off
    G28 X0 Y0
    G90
    #G1 X5 Y5 F6000      ; Move near X/Y Home
    {% if printer.toolhead.position.y+30 < 300 %}
        G91
        G1 Z+30 F6000   ; Lots of Z axis room to clear print
    {% endif %}
    M84                 ; Disable steppers
```


