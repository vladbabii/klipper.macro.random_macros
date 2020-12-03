```
[printer]
max_velocity:               500
max_accel:                  5000
max_accel_to_decel:         5000
111

```
[gcode_macro M204]
rename_existing:            M204.1
default_parameter_F:        0.75
gcode:
    {% if 'S' in params %}
        SET_VELOCITY_LIMIT ACCEL={ S } ACCEL_TO_DECEL={ S|float * F|float }
    {% endif %}

[gcode_macro M205]
gcode:
    {% if 'X' in params %}
        SET_VELOCITY_LIMIT SQUARE_CORNER_VELOCITY={ X }
    {% endif %}
```

---

