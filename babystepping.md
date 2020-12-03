```
######################################################################
# babystep
######################################################################


#    Macro to Babystep Up 0.01mm
[gcode_macro babystep_up3]
gcode:
    SET_GCODE_OFFSET Z_ADJUST=0.01 MOVE=1

#    Macro to Babystep Down 0.01mm
[gcode_macro babystep_down3]
gcode:
    SET_GCODE_OFFSET Z_ADJUST=-0.01 MOVE=1

#    Macro to Babystep Up 0.02mm
[gcode_macro babystep_up]
gcode:
    SET_GCODE_OFFSET Z_ADJUST=0.02 MOVE=1

#    Macro to Babystep Down 0.02mm
[gcode_macro babystep_down]
gcode:
    SET_GCODE_OFFSET Z_ADJUST=-0.02 MOVE=1

#    Macro to Babystep Up 0.05mm
[gcode_macro babystep_up2]
gcode:
    SET_GCODE_OFFSET Z_ADJUST=0.05 MOVE=1

#    Macro to Babystep Down 0.05mm
[gcode_macro babystep_down2]
gcode:
    SET_GCODE_OFFSET Z_ADJUST=-0.05 MOVE=1
```

