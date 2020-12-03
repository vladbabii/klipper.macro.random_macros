```
[gcode_macro silent]
gcode:
        SET_TMC_FIELD STEPPER=stepper_x FIELD=TPWMTHRS VALUE=00000000 ; set threshold to 0 so it in stealthchop all the time
        SET_TMC_FIELD STEPPER=stepper_x FIELD=en_spreadCycle VALUE=0 ;enable stealthchop
        SET_TMC_FIELD STEPPER=stepper_x1 FIELD=TPWMTHRS VALUE=00000000 ; set threshold to 0 so it in stealthchop all the time
        SET_TMC_FIELD STEPPER=stepper_x1 FIELD=en_spreadCycle VALUE=0 ;enable stealthchop
        SET_TMC_FIELD STEPPER=stepper_y FIELD=TPWMTHRS VALUE=00000000 ; set threshold to 0 so it in stealthchop all the time
        SET_TMC_FIELD STEPPER=stepper_y FIELD=en_spreadCycle VALUE=0 ;enable stealthchop
        SET_TMC_FIELD STEPPER=stepper_y1 FIELD=TPWMTHRS VALUE=00000000 ; set threshold to 0 so it in stealthchop all the time
        SET_TMC_FIELD STEPPER=stepper_y1 FIELD=en_spreadCycle VALUE=0 ;enable stealthchop
        M220 S20 ;set print speed to 20%
```
