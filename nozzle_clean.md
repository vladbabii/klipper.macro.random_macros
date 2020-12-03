```
...
{% for n in range(6) %}
  nozzle_clean_path        #run macro for nozzle clean
{% endfor %}
...

[gcode_macro nozzle_clean_path]
########### Change this ############
default_parameter_Y: 50
########### Gcode ############
gcode:
    G0 Y{Y} F9000                #scrub
    G0 Y-{Y} F9000                #scrub
````
