auto power off after idle timeout - https://pastebin.com/KiqUMG6w

```
#Here is an auto-off script
#It works by starting a delayed macro from the idle_status macro. It will then periodically check if printer is still idle and extruder/bed are below certain temps. If all conditions are met, the user will get a warning and have one minute to intervene, after the one minute the printer commits sudoku
#I would have liked to have the timers configured in a neat list of variables, but I don't know how to do that when they need to be global in scope (because there are multiple macro's needed).
 
#Feel free to change the values to your liking.
 
#######################################################################################################
#BE SURE TO CHANGE "PSU" TO YOUR DEVICE IN THE [delayed_gcode delayed_shutdown] SECTION AT THE BOTTOM!#
#######################################################################################################
 
#Greetings, Harm B., AKA: @Appel
 
[idle_timeout]
gcode:
    { action_respond_info("Printer status is now 'Idle'") }
    TURN_OFF_HEATERS
    M84
    UPDATE_DELAYED_GCODE ID=auto_off DURATION=1800 #wait this long before first periodic check
 
timeout: 600
#   Idle time (in seconds) to wait before running the above G-Code
#   commands. The default is 600 seconds
 
[gcode_macro INTERRUPT_AUTO_SHUTDOWN]
gcode:
     UPDATE_DELAYED_GCODE ID=delayed_shutdown DURATION=0 #stop delayed_shutdown from getting executed
     { action_respond_info("Auto shutdown averted") }
     UPDATE_DELAYED_GCODE ID=auto_off DURATION=1800 #start checking again
 
[delayed_gcode auto_off]
#This macro gets called delayed by the idle_timeout macro and checks for a few conditions before triggering a shutdown
#Conditions are: printer state is 'Idle', extruder temp is below or equal to 45°C, bed temp is below or equal to 60°C
 
initial_duration: 1800 
#klipper starts in the 'Idle' state but does not run the idle_timeout macro at startup,
#so there must be a non-zero value set here or the printer will not auto shutdown when
#it has not had another state than 'Idle' since the last klipper startup
 
gcode:
 
    { action_respond_info(printer.idle_timeout.state) }
    {% if printer.idle_timeout.state == "Idle" %} 
        {% if printer.extruder.temperature <= 45 and printer.heater_bed.temperature <= 60 %}
            { action_respond_info("Auto shutdown in 60 seconds, execute INTERRUPT_AUTO_SHUTDOWN to stop this.") } #warn user
            UPDATE_DELAYED_GCODE ID=delayed_shutdown DURATION=60
 
        {% else %}
            UPDATE_DELAYED_GCODE ID=auto_off DURATION=900 #printer was idle, but other conditions not met, check again later
        {% endif %}
 
    #{% else %}
    #     { action_respond_info("Printer no longer idle") } #printer state was polled and no longer idle, stop repeating this routine. 
 
    {% endif %}
 
[delayed_gcode delayed_shutdown]
initial_duration: 0 
gcode:
    {% if printer.extruder.temperature <= 45 and printer.heater_bed.temperature <= 60 and printer.idle_timeout.state == "Idle" %}
        { action_respond_info("Shutting down now...") }
        { action_call_remote_method("set_device_power", device="PSU", state="off") } #CHANGE "PSU" TO YOUR DEVICE!
    {% else %}
        { action_respond_info("Auto shutdown averted") } #user did something to alter printer state
        UPDATE_DELAYED_GCODE ID=auto_off DURATION=900 #start checking again
    {% endif %}
```
