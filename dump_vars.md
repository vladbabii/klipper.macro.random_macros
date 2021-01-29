```
[gcode_macro DUMP_VARS]
gcode:
   {% for name1 in printer %}
      {% for name2 in printer[name1] %}
         { action_respond_info("printer['%s'].%s = %s" % (name1, name2, printer[name1][name2])) }
      {% endfor %}
   {% endfor %}

[gcode_macro DUMP_CONFIG_VARS]
gcode:
   {% for name1 in printer['configfile'].config %}
      #{% for name2 in printer[name1] %}
         { action_respond_info("printer['configfile'].%s" % (name1)) }
      #{% endfor %}
   {% endfor %}
```

```
[gcode_macro DUMP_PARAMETERS]
gcode:
  {% set parameters = namespace(output = '') %}
  {% for name1 in printer %}
    {% for name2 in printer[name1] %}
      {% set donotwant = ['bed_mesh','configfile'] %}
      {% if name1 is not in donotwant %}
        {% set param = "printer['%s'].%s = %s" % (name1, name2, printer[name1][name2]) %}
        {% set parameters.output = parameters.output +  param + "\n" %}
      {% endif %}
      {% else %}
        {% set param = "printer['%s'] = %s" % (name1, printer[name1]) %}
        {% set parameters.output = parameters.output +  param + "\n" %}
    {% endfor %}
  {% endfor %}
  {action_respond_info(parameters.output)}
```
