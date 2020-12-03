```
[gcode_macro load_filament]
default_parameter_EXTRUDER: 200
gcode:
    {% if printer.toolhead.status == "Ready" %}
        G90
        G0 X410 Y40                #move to area where can easily load filament
        M109 S{EXTRUDER}        #set hotend temperature and wait
        M83                        #relative positioning on extruder    
        G0 E160 F400              #prime extruder
        G92 E0
        UPDATE_DELAYED_GCODE ID=notify_extruder_load DURATION=10
    {% else %}
        { printer.gcode.action_respond_info("Load Filament is disabled while printing!") }
    {% endif %}


#    Macro to Unload Filament
[gcode_macro unload_filament]
default_parameter_EXTRUDER: 200
gcode:
    {% if printer.toolhead.status == "Ready" %}
        G0 X410 Y40                #move to area where can easily load filament
        M109 S{EXTRUDER}        #set hotend temperature and wait    
        M83                        #relative positioning on extruder
        G0 E15 F400            #extrude filament to get better blob on end
        G0 E-130 F1000          #retract additional filament to move out of melt zone
        G92 E0
        UPDATE_DELAYED_GCODE ID=notify_extruder_reload DURATION=10
    {% else %}
        { printer.gcode.action_respond_info("Unload Filament is disabled while printing!") }
    {% endif %}
```

---


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

```
[gcode_macro load_filament]
default_parameter_EXTRUDER: 200
default_parameter_X: 410
default_parameter_Y: 40
default_parameter_Z: 10
default_parameter_E: 160
gcode:
    {% if printer.toolhead.status == "Ready" %}
        G90
        G0 X{X} Y{Y}                #move to area where can easily load filament
        M109 S{EXTRUDER}        #set hotend temperature and wait
        M83                        #relative positioning on extruder    
        G0 E{E} F400              #prime extruder
        G92 E0
        UPDATE_DELAYED_GCODE ID=notify_extruder_load DURATION=10
    {% else %}
        { printer.gcode.action_respond_info("Load Filament is disabled while printing!") }
    {% endif %}
```

---

```
######################################################################
# Filament Change
######################################################################

# M600: Filament Change. This macro will pause the printer, move the
# tool to the change position, and retract the filament 50mm. Adjust
# the retraction settings for your own extruder. After filament has
# been changed, the print can be resumed from its previous position
# with the "RESUME" gcode.

[gcode_macro M600]
default_parameter_X: 410
default_parameter_Y: 40
default_parameter_Z: 10
gcode:
    {% if printer.toolhead.status == "Ready" %}
        # do nothing
    {% else %}
        M117 Filament Change
        SAVE_GCODE_STATE NAME=M600_state
        PAUSE
        G91
        G1 E-5 F4000
        G1 Z{Z}
        G90
        G1 X{X} Y{Y} F3000
        G0 E30 F500            #extrude filament to get better blob on end
        G0 E-130 F600          #retract additional filament to move out of melt zone
        G92 E0
        #RESTORE_GCODE_STATE NAME=M600_state
    {% endif %}

#    Use this command to load filament during a mid print filament swap    
[gcode_macro SWAP_RESUME] 
gcode:
     M117 Printing...
    RESTORE_GCODE_STATE NAME=M600_state
    resume
```
