```
[gcode_macro _SELECT_PA]
gcode:
  # set default parameter values
  {% set nozzle = params.NOZZLE|default(0.4)|float %}
  {% set filament = params.FILAMENT|default('None')|string %}
  #####   Pressure Advance values for different filaments & nozzles #####
  {% set pa_def = [('ESUN_ABS+_Black', 0.4, 0.05),
                   ('KVP_ABS_FL_Blue', 0.4, 0.05)] %}
  #######################################################################
  {% set elem_cnt = pa_def|length %}
  {% set ns = namespace(index = elem_cnt) %}
  {% for index in range(elem_cnt) %}
     {% if pa_def[index][0]|lower == filament|lower and pa_def[index][1]|float == nozzle %}
       {% set ns.index = index %}
    {% endif %}
  {% endfor %}
  {% if ns.index < elem_cnt %}
    {% set elem_filament = pa_def[ns.index][0]|string %}
    {% set elem_nozzle = pa_def[ns.index][1]|float %}
    {% set elem_pa = pa_def[ns.index][2]|float %}
  {% else %}
    {% set elem_filament = 'default' %}
    {% set elem_nozzle = 0.4 %}
    {% set elem_pa =  printer.configfile.settings['extruder'].pressure_advance|float %}
  {% endif %}
  SET_PRESSURE_ADVANCE ADVANCE={elem_pa}
  {action_respond_info("PRESSURE_ADVANCE:
                        FILAMENT: %s
                        NOZZLE: %1.1f
                        VALUE: %.4f" % (elem_filament, elem_nozzle, elem_pa))}
```
