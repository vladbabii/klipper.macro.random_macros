```
[gcode_macro HEAT_SOAK]
#uncomment HEAT_SOAK lines in PRINT_START to enable
gcode:
    G0 X60 Y60 Z10                                   ; move toolhead to centre
    PAUSE
    M106 S255                                        ; run cooling fans at full power
    M117
    UPDATE_DELAYED_GCODE ID=SOAK_TIME DURATION=600   ; resume after 300 seconds

[delayed_gcode SOAK_TIME]
gcode:
    RESUME
    M107                                             ; turn off cooling fans

[gcode_macro SKIP_HEAT_SOAK]
gcode:
    RESUME
    UPDATE_DELAYED_GCODE ID=SOAK_TIME DURATION=1
```
