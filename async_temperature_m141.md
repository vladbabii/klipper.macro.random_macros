```
[gcode_macro M141]
default_parameter_S:        0
variable_chamber_target:    0
gcode:   
    {% if params.S|float > 0 %}
      SET_GCODE_VARIABLE MACRO=M141 VARIABLE=chamber_target VALUE={ S }
      UPDATE_DELAYED_GCODE ID=chamber_timer DURATION=1
    {% else %}
      SET_GCODE_VARIABLE MACRO=M141 VARIABLE=chamber_target VALUE=0
      UPDATE_DELAYED_GCODE ID=chamber_timer DURATION=0
    {% endif %} 
  
[gcode_macro manage_chamber_fan]
gcode:
    {% set Ct = printer['temperature_sensor chamber'].temperature|float|round(2) %}
    {% set Cr = printer["gcode_macro M141"].chamber_target|float|round(2) %}

    {% if Cr > 0 and Ct  > Cr %}
        SET_FAN_SPEED FAN=chamber_fan SPEED=1
    {% else %}
        SET_FAN_SPEED FAN=chamber_fan SPEED=0
    {% endif %} 

    UPDATE_DELAYED_GCODE ID=chamber_timer DURATION=1

[delayed_gcode chamber_timer]
initial_duration: 1
gcode:
    manage_chamber_fan
```
