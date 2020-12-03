```
[gcode_macro select_build_plate]
variable_build_plate: 0
default_parameter_I:        0
gcode:
    { action_respond_info("Specify Build Plate: ") }
    {% if 'I' in params %}
        SET_GCODE_VARIABLE MACRO=variable_build_plate VARIABLE=build_plate VALUE={I}    
    {% else %}
        UPDATE_DELAYED_GCODE ID=prompt_build_plate DURATION=1
    {% endif %}
    
[delayed_gcode prompt_build_plate]
initial_duration: 1
gcode:
    {% if  printer["gcode_macro prompt_build_plate"].build_plate == 0 %}
        UPDATE_DELAYED_GCODE ID=prompt_build_plate DURATION=1
    {% elif printer["gcode_macro prompt_build_plate"].build_plate == 1 %}
        #do some gcode offset stuff or whatever
    {% elif printer["gcode_macro prompt_build_plate"].build_plate == 2 %}
        #do some gcode offset stuff or whatever
    {% endif %}

```
