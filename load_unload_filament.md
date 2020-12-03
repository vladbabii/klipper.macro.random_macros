```
######################################################################
# load / unload filament
######################################################################


#    Macro to Load Filament
[gcode_macro load_filament]
########### Change this ############
default_parameter_EXTRUDER: 210
default_parameter_X: 10            #park position
default_parameter_Y: 40                #park position
default_parameter_Z: 10                #park position
default_parameter_E: 160
########### Gcode ############
gcode:
        G90
        G0 X{X} Y{Y}                #move to area where you can easily load filament
        M109 S{EXTRUDER}            #set hotend temperature and wait
        M83                         #relative positioning on extruder    
        G0 E{E} F400                #prime extruder
        G92 E0

#    Macro to Unload Filament
[gcode_macro unload_filament]
########### Change this ############
default_parameter_EXTRUDER: 210
default_parameter_X: 10
default_parameter_Y: 40
default_parameter_Z: 10
default_parameter_E: 160
########### Gcode ############
gcode:
        G0 X{X} Y{Y}                #move to area where you can easily load filament
        M109 S{EXTRUDER}            #set hotend temperature and wait    
        M83                         #relative positioning on extruder
        G0 E15 F400                 #extrude filament to get better blob on end
        G0 E{E} F1000               #retract additional filament to move out of melt zone
        G92 E0
```

---

