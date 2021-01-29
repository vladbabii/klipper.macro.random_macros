```
[pause_resume]

[gcode_macro WAIT_PWM]
gcode:
  {% if printer['heater_bed'].power|float > 0.4 %}
    PAUSE
    UPDATE_DELAYED_GCODE ID=DELAY_PWM DURATION=1
  {% else %}
    RESUME
  {% endif %}

[delayed_gcode DELAY_PWM]
gcode:
  {% if printer['heater_bed'].power|float > 0.4 %}
    UPDATE_DELAYED_GCODE ID=DELAY_PWM DURATION=1
  {% else %}
    WAIT_PWM
  {% endif %}
```
