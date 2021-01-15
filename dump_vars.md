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
