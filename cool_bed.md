```
[gcode_macro COOL_BED]
default_parameter_DELAY: 30
gcode:
        M118 {printer.heater_bed.temperature}
        {% for t in range(printer.heater_bed.temperature |int, 30, -1) %}
                M140 S{t}
                G4 P{(params.DELAY |float * 1000) |int }
        {% endfor %}
        M140 S0
```
