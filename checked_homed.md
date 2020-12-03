```
[gcode_macro check_homed]
default_parameter_AXIS: X
gcode:
    {% if AXIS in printer.toolhead.homed_axes %}
    RESPOND MSG="{AXIS} is homed"
    {% else %}
    RESPOND MSG="{AXIS} is not homed"
    {% endif %}
    
[gcode_macro RESPOND]
default_parameter_MSG: ""
gcode: {printer.gcode.action_respond_info(MSG)}
```
