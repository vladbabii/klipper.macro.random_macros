```
[gcode_macro G32]
gcode:
  BED_MESH_CLEAR
  {% if "z" not in printer.toolhead.homed_axes %} ; G28 Home if needed
    M117 "Homing"
    G28
  {% endif %}
  M117 "Leveling gantry."
  QUAD_GANTRY_LEVEL                                      ; Level Gantry
  M117 "Clean Z home"
  G28 Z                                             ; Home Z 
  G0 X150 Y150 Z20 F6000
```

```
# Quad Gantry Level
[gcode_macro QUAD_GANTRY_LEVEL]
rename_existing: BASE_QUAD_GANTRY_LEVEL
gcode:
  BED_MESH_CLEAR                      ; clear bed mesh
  CG28                                    ; home all axes if not already
  M117 Gantry Leveling
  BASE_QUAD_GANTRY_LEVEL            ; level gantry
  G28 Z                                    ; rehome z
  M117 V2.312 ~voron~
```

```
[gcode_macro QUAD_GANTRY_LEVEL]
rename_existing: BASE_QUAD_GANTRY_LEVEL
variable_qgl_done: 0
gcode:
    ## reduce current of Z motors
    SET_TMC_CURRENT STEPPER=stepper_z  CURRENT=0.3 HOLDCURRENT=0.3
    SET_TMC_CURRENT STEPPER=stepper_z1 CURRENT=0.3 HOLDCURRENT=0.3
    SET_TMC_CURRENT STEPPER=stepper_z2 CURRENT=0.3 HOLDCURRENT=0.3
    SET_TMC_CURRENT STEPPER=stepper_z3 CURRENT=0.3 HOLDCURRENT=0.3
    BASE_QUAD_GANTRY_LEVEL
    G28 Z
    ## return to org current settings
    SET_TMC_CURRENT STEPPER=stepper_z  CURRENT={ printer.configfile.config["tmc2209 stepper_z"]["run_current"] }  HOLDCURRENT={ printer.configfile.config["tmc2209 stepper_z"]["hold_current"] }
    SET_TMC_CURRENT STEPPER=stepper_z1 CURRENT={ printer.configfile.config["tmc2209 stepper_z1"]["run_current"] } HOLDCURRENT={ printer.configfile.config["tmc2209 stepper_z1"]["hold_current"] }
    SET_TMC_CURRENT STEPPER=stepper_z2 CURRENT={ printer.configfile.config["tmc2209 stepper_z2"]["run_current"] } HOLDCURRENT={ printer.configfile.config["tmc2209 stepper_z2"]["hold_current"] }
    SET_TMC_CURRENT STEPPER=stepper_z3 CURRENT={ printer.configfile.config["tmc2209 stepper_z3"]["run_current"] } HOLDCURRENT={ printer.configfile.config["tmc2209 stepper_z3"]["hold_current"] } 
    SET_GCODE_VARIABLE MACRO=QUAD_GANTRY_LEVEL VARIABLE=qgl_done VALUE=1 
```
