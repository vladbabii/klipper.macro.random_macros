```
[gcode_macro insert_item]
default_parameter_X: 300
default_parameter_Y: -20
default_parameter_Z: 10
gcode:
    SAVE_GCODE_STATE NAME=M600_state
    PAUSE
    G1 X{X} Y{Y} F3000
    action_needed
    paused
```

---
