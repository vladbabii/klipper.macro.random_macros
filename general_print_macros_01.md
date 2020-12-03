```
[gcode_macro TEST_var]
variable_bed_temp: 60
variable_e0_temp: 215
variable_e0_layer0_temp: 230
gcode:
  M117 {bed_temp}
 
[gcode_macro TEST_param]
gcode:
  { printer.gcode.action_respond_info("TEST_param") }
    M117 {params.T}
 
#SET_GCODE_VARIABLE MACRO=TEST_var VARIABLE=bed_temp VALUE=60
#SET_GCODE_VARIABLE MACRO=TEST_var VARIABLE=e0_temp VALUE=240
#SET_GCODE_VARIABLE MACRO=TEST_var VARIABLE=e0_layer0_temp VALUE=240
#SET_GCODE_VARIABLE MACRO=TEST_var VARIABLE=bed_layer0_temp VALUE=60
 
[gcode_macro TEST_homed]
gcode:
 
 
[gcode_macro PRINT_START]
variable_bed_temp: 60
variable_e0_temp: 215
variable_e0_layer0_temp: 230
variable_bed_layer0_temp: 65
#   Use PRINT_START for the slicer starting script - PLEASE CUSTOMISE THE SCRIPT FOR YOUR SLICER OF CHOICE
gcode:
#    M118 {e0_layer0_temp}
#    M118 {bed_layer0_temp}
    #set bed and nozzle temps
    #G28 x0 y0
    M117 Heating...
    M104 S{e0_layer0_temp}
    M140 S{bed_layer0_temp}
    M400
    G28
    M400
    ENTER_PARKING
    #set temps and wait
    M109 S{e0_layer0_temp}
    M190 S{bed_layer0_temp}
    EXIT_PARKING
    #G28 Y0
    #G1 Y80 F16000
    #G28
    M117 Running Z-tilt
    z_tilt_adjust
    M117 Homing...                 ; display message
    G28 Z0 #1/30
#1/30    G1 Z5 F5000 ; lift nozzle
 
    BED_MESH_CALIBRATE
#   Move to the cleaning area
    M117 Wiping Nozzle
    ENTER_PARKING
   G28 Z0
#    G1 Z20 F300                   ; move nozzle away from bed
#    G1 X100 Y40 F4200                ; move nozzel to center front
#    M117 Preheat (Print)           ; display message
    M117
    EXIT_PARKING
 
[gcode_macro PRINT_END]
#   Use PRINT_END for the slicer ending script - PLEASE CUSTOMISE THE SCRIPT FOR YOUR SLICER OF CHOICE
gcode:
    M400                           ; wait for buffer to clear
 
    G92 E0                         ; zero the extruder
    G1 E-3.0 F3600                 ; retract
    G91                            ; relative positioning
    G0 Z1.00 X20.0 Y20.0 F20000    ; move nozzle to remove stringing
    M104 S0                        ; turn off hotend
    M140 S0                        ; turn off bed
    M106 S0                        ; turn off fan
    G1 Z10 F1500                   ; move nozzle up 20mm
    G90                            ; absolute positioning
 #   G0  X375 Y415 F4200            ; park nozzle at rear
    ENTER_PARKING
    G1 Z410 F1200
    M117 Finished!                 ; display message
 
[gcode_macro ENTER_PARKING]
gcode:
    M400
    G90
#    G1 Y80 F16000
    G1 X320 F16000
    G1 Y0 F16000
    G1 X351 F16000
    #G1 X335 Y0 F16000
[gcode_macro EXIT_PARKING]
gcode:
    G90
    G1 X365 F20000
    G1 X375 F20000
    G1 X381 F20000
    G1 X375 F20000
    G1 X381 F20000
    G1 X375 F20000
    G1 X381 F20000
    G1 X335 F20000
    G1 Y0 F20000
    G1 Y19 F20000
    G1 Y0 F20000
    G1 Y19 F20000
    G1 Y0
    G1 Y100 F16000
 
 
[gcode_macro UNLOAD_FILAMENT]
gcode:
    G90                            ; absolute positioning
#    G28 X0 Y0
#    G1 X100 Y0 F3200
    ENTER_PARKING
    M83                            ; relative extrusion
    G1 E80 F300
    G1 E-300 F2000
    M82                            ; absolute extrusion
    M300 P2000
 
[gcode_macro LOAD_FILAMENT]
gcode:
    M300 P1000
    G90                            ; absolute positioning
#    G28 X0 Y0
    ENTER_PARKING
    M83                            ; relative extrusion
    G1 E50 F2000
    G1 E80 F300
#    G1 E50 F150
    M82                            ; absolute extrusion
 
[gcode_macro HOME_ANDDROPBED]
gcode:
    M117 Home All
    G28
    M117 Dropping Bed
    G1 Z350 F2400
    M117 Done
 
[gcode_macro ON_CONNECT]
gcode:
    M117 Home All
    G28
    M117 Adjusting Z-tilt
    Z_TILT_ADJUST
#    M117 Home All Again
#    BED_MESH_CALIBRATE
#    G28
    M117 Park Nozzle
    G1 Z100 F1200
#    M117 Mesh Calibration#    BED_MESH_CALIBRATE
    M117 Done
    M117
#    save_config
 
 ```
