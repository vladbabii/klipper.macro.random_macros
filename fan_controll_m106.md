```
[gcode_macro fan_config]
variable_toolindex: 0
variable_speed: 0
gcode:
   RESPOND PREFIX="info" MSG="Macro > Fan config > ok"

[gcode_macro M106]
gcode:
  {% set fanspeed = 255 %}
  {% if params.S is defined %}
    {% set fanspeed = params.S|int %}
  {% endif %}
  {% if fanspeed < 0 %}
    {% set fanspeed = 0 %}
  {% endif %}
  {% if fanspeed > 255 %}
    {% set fanspeed = 255 %}
  {% endif %}
  RESPOND PREFIX="info" MSG="Macro > M106 > speed {fanspeed}"
  FANSPEED SPEED={fanspeed}

[gcode_macro M107]
gcode:
  RESPOND PREFIX="info" MSG="Macro > M107 > speed 0"
  FANSPEED SPEED=0
  
[gcode_macro FANSPEED]
default_parameter_SPEED: 255
gcode:
  RESPOND PREFIX="info" MSG="Macro > fanspeed > SET FAN to { params.SPEED|int }"
  SET_GCODE_VARIABLE MACRO=fan_config VARIABLE=speed VALUE={ params.SPEED|int }
  
  {% if params.SPEED is defined %}

    {% if params.SPEED|int == 255 %}
      {% set realspeed = 1 %}
    {% else %}
      {% if params.SPEED|int == 0 %}
        {% set realspeed = 0 %}
      {% else %}
        {% set realspeed = 0.003921*params.SPEED|int %}
      {% endif %}
    {% endif %}

  {% else %}

    {% set realspeed = 0 %}

  {% endif %}
  
  RESPOND PREFIX="info" MSG="Macro > fanspeed > SET FAN realspeed to {realspeed}"
  SET_PIN PIN=fan_{printer.toolhead.extruder} VALUE={realspeed}
```

