```
#Pause/Resume Functionality
[pause_resume]

#Wait for chamber temp, kick off wait loop if not already at temp
[gcode_macro M191]
default_parameter_S: 0
variable_chambertargettemp: 0
gcode:
    SET_GCODE_VARIABLE MACRO=M191 VARIABLE=chambertargettemp VALUE={S}                   ; set target temp for reference outside of the macro (for the loop)
    
    {% if not printer["temperature_sensor chamber_temp"].temperature >= S|int %}    ; ##IF CHAMBER TEMP IS NOT ALREADY REACHED##
        { action_respond_info("Chamber not at temp yet, pausing...") }
            {% if not printer.pause_resume.is_paused %}
                PAUSE                                                                   ; pause if not already paused
            {% endif %}
        UPDATE_DELAYED_GCODE ID=M191-Wait DURATION=5                                   ; start wait loop
    {% else %}
        { action_respond_info("Chamber at or above temp, continuing...") }             ; ##IF CHAMBER TEMP IS ALREADY REACHED##
        UPDATE_DELAYED_GCODE ID=M191-Wait DURATION=0                                   ; break wait loop if it happens to be running already (shouldn't be)
        {% if printer.pause_resume.is_paused %}
            RESUME                                                                       ; resume if paused (shouldn't be)
        {% endif %}
    {% endif %}
    
#This part will loop until the desired chamber temp is reached, then resume the print
[delayed_gcode M191-Wait]
gcode:
    {% if printer["temperature_sensor chamber_temp"].temperature >= printer["gcode_macro M191"].chambertargettemp|int %} ; ##IF CHAMBER TEMP IS REACHED##
        { action_respond_info("Chamber at or above temp, continuing...") }
            {% if printer.pause_resume.is_paused %}
                RESUME                                                                                                        ; break loop, resume print
            {% endif %}
    {% else %}                                                                                                             ; ##IF CHAMBER TEMP IS NOT YET REACHED##
        { action_respond_info("Chamber not at temp yet, waiting...") }
        UPDATE_DELAYED_GCODE ID=M191-Wait DURATION=5                                                                        ; continue waiting loop
    {% endif %}
```
