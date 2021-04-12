```
[gcode_macro mcu_version]
gcode:
  {action_respond_info(printer.mcu.mcu_version)}

[gcode_macro mcu_build_versions]
gcode:
  {action_respond_info(printer.mcu.mcu_build_versions)}
```

```
[gcode_macro M115.1]
rename_exsisting: M115
gcode:
  {action_respond_info(printer.mcu.mcu_version)}
{action_respond_info(printer.mcu.mcu_build_versions)}
```

```
[gcode_macro DUMP_MCU_VER]
gcode:
  {% set parameters = namespace(output = 'mcu version: \n') %}
  {% for name1 in printer %}
    {% for name2 in printer[name1] %}
      {% set show = ['mcu_version'] %}
      {% if name2 is in show %}
        {% set param = "%s: %s" % (name1, printer[name1][name2]) %}
        {% set parameters.output = parameters.output +  param + "\n" %}
      {% endif %}
    {% endfor %}
  {% endfor %}
  {action_respond_info(parameters.output)}
```
