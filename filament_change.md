```
######################################################################
# Filament Change
######################################################################


# M600: Filament Change. This macro will pause the printer, move the
# tool to the change position, and retract the filament 130mm. Adjust
# the retraction settings for your own extruder. After filament has
# been changed, the print can be resumed from its previous position
# with the "RESUME" gcode.

[gcode_macro M600]
########### Change this ############
default_parameter_X: 410            #park position
default_parameter_Y: 40                #park position
default_parameter_Z: 10                #park position
default_parameter_E: -130            #retract dist
########### Gcode ############
gcode:
        SAVE_GCODE_STATE NAME=M600_state
        PAUSE
        G91
        G1 E-5 F4000
        G1 Z{Z}
        G90
        G1 X{X} Y{Y} F3000        ;park position
        G0 E10 F500                ;extrude filament to get better blob on end
        G0 E{E} F600             ;retract additional filament to move out of melt zone
        G92 E0

#    Use this command resume during a mid print filament swap (DONT USE OCTO/MAINSAIL/DWC RESUME)
[gcode_macro SWAP_RESUME] 
gcode:
    RESTORE_GCODE_STATE NAME=M600_state
    resume
```
