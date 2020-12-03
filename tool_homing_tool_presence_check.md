```
[gcode_macro G28]
rename_existing:    G28.1
gcode:
    {%set p=[] %}
    {% for key in params %}
        {% if key != 'G' %}
            {% set p = p.append(key + params[key])  %}
        {% endif %}
    {% endfor %}
    G28.1 { p|join(" ") }

This snippet is an example of intercepting a GCode and passing all of its params to the renamed gcode...
In my case i'm using it to  prevent people from homing with the endstop switch is blocked by a tool..
[gcode_macro G28]
rename_existing:    G28.1
gcode:
    {%set p=[] %}
    {% for key in params %}
        {% if key != 'G' %}
            {% set p = p.append(key + params[key])  %}
        {% endif %}
    {% endfor %}
    {% if not printer["gcode_macro DOCK_INIT"].tool_present%}
        G28.1 { p|join(" ") }
    {% else %}
    { printer.gcode.action_respond_info("You attemped to home while a tool is present") }
    {% endif %}
```

