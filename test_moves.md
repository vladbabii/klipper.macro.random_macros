```
[gcode_macro test_moves]
default_parameter_I:    10
gcode:

    {% set iters = params.I|int %}
    {% set tool = printer.toolhead %}
    {% set axisMax     = {'x':config.stepper_x.position_max|float,
                          'y':config.stepper_y.position_max|float,
                          'z':config.stepper_z.position_max|float} %}

    {% set axisMin     = {'x':config.stepper_x.position_min|float,
                          'y':config.stepper_y.position_min|float,
                          'z':config.stepper_z.position_min|float} %}
                          
    {% for f in range(1,iters) %}
        {% set a = range(0, tool.max_accel)|random %}
        {% set f = range(0, tool.max_velocity)|random * 60 %}
        {% set x = range(axisMin.x, axisMax.x)|random %}
        {% set y = range(axisMin.y, axisMax.y)|random %}
        M204 S{a}
        G1 X{x} Y{y} F{f}
    {% endfor %}
```
