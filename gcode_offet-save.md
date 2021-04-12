```
[save_variables]
filename: ~/savedVariables.cfg

[gcode_macro SET_GCODE_OFFSET]
rename_existing: _SET_GCODE_OFFSET
gcode:
    {% if printer.save_variables.variables.gcode_offsets %}
        {% set offsets = printer.save_variables.variables.gcode_offsets %}
    {% else %} 
        {% set offsets = {'x': None,'y': None,'z': None} %}
    {% endif %}
    
    {% set ns = namespace(offsets={'x': offsets.x,'y': offsets.y,'z': offsets.z}) %}
    
    _SET_GCODE_OFFSET {% for p in params %}{'%s=%s '% (p, params[p])}{% endfor %}

    {%if 'X' in params %}{% set null = ns.offsets.update({'x': params.X}) %}{% endif %}
    {%if 'Y' in params %}{% set null = ns.offsets.update({'y': params.Y}) %}{% endif %}
    {%if 'Z' in params %}{% set null = ns.offsets.update({'z': params.Z}) %}{% endif %}
    SAVE_VARIABLE VARIABLE=gcode_offsets VALUE="{ns.offsets}"

[delayed_gcode LOAD_GCODE_OFFSETS]
initial_duration: 2
gcode:  
    {% if printer.save_variables.variables.gcode_offsets %}
        {% set offsets = printer.save_variables.variables.gcode_offsets %}

        _SET_GCODE_OFFSET {% for axis, offset in offsets.items()
            if offsets[axis] %}{ "%s=%s " % (axis, offset) }{% endfor %}

        { action_respond_info("Loaded gcode offsets from saved variables [%s]" % (offsets)) }
    {% endif %}|
```
