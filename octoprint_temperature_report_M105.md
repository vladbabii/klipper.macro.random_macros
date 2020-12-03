```
[respond]
default_type:               echo
default_prefix:             echo:

[gcode_macro M105]
rename_existing:            M105.1
gcode:  

   
    {% set Bt = printer.heater_bed.temperature|float|round(2) %}
    {% set Br = printer.heater_bed.target|float|round(2) %}
    {% set Ct = printer['temperature_sensor chamber'].temperature|float|round(2) %}
    {% set Cr = printer["gcode_macro M141"].chamber_target|float|round(2) %}
    {% set Tt = printer.extruder.temperature|float|round(2) %}
    {% set Tr = printer.extruder.target|float|round(2) %}
    
    RESPOND PREFIX=B: MSG="{'%s /%s C:%s /%s T0:%s /%s' % (Bt, Br, Ct, Cr, Tt, Tr)}"
```
