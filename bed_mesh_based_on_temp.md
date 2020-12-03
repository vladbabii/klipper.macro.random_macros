```
# Store bed meshes based on bed temperature set point
[gcode_macro BEDMESHPROCEDURE]
gcode:
    BED_MESH_CLEAR
    G28
    QUAD_GANTRY_LEVEL
    G28
    BED_MESH_CALIBRATE
    BED_MESH_PROFILE SAVE={printer.heater_bed.target}C
    SAVE_CONFIG

# Load bed meshes based on bed temperature set point
[gcode_macro BEDMESHLOAD]
gcode:
    BED_MESH_PROFILE LOAD={printer.heater_bed.target}C
    G28
```
