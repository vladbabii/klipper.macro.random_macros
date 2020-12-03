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

---

```
[gcode_macro START_PRINT]
default_parameter_BED_TEMP: 50
default_parameter_EXTRUDER_TEMP: 170
gcode:
  M140 S{BED_TEMP} ; set bed temp
  M104 S170 ; Preheat Extruder
  SET_GCODE_OFFSET Z=0.0; (Reset the G-Code Z offset)
  G28 X0 Y0 Z0 ; Home XY
  G21 ; metric values
  G90 ; absolute positioning
  M82 ; set extruder to absolute mode
  M107 ; start with the fan off
  M109 S170 ; Preheat extruder and wait
  brush
  G28 Z0 ; Home Z again
  BED_MESH_PROFILE LOAD=DEFAULT
  G0 X0 Y-12
  status
  M190 S{BED_TEMP}; heat bed (wait)
  M109 S{EXTRUDER_TEMP}; heat hotend (wait)
  G92 E0 ; Zero the extruder
  brush
  G0 E10 F500 ; prime the nozzle
  G0 Z0
  G0 X0 Y0
  G1 Y60 E8 F500 ; Draw a priming/wiping line to the rear
  G0 X1 Z0.15
  G1 Y10 E16 F500 ; draw more priming/wiping
  G92 E0 ; Zero the extruder

[gcode_macro END_PRINT]
gcode:
  heaters_off
  G91
  G0 Z-.5 F2000
  G0 Z5 E-5 F800
  G90
  G28 X0 Y295 F500
  motors_off
  BED_MESH_CLEAR
```

---


