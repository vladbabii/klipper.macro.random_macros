```
[gcode_macro mcu_version]
gcode:
  {action_respond_info(printer.mcu.mcu_version)}

[gcode_macro mcu_build_versions]
gcode:
  {action_respond_info(printer.mcu.mcu_build_versions)}
```

```
[gcode_macro M115.1]
rename_exsisting: M115
gcode:
  {action_respond_info(printer.mcu.mcu_version)}
{action_respond_info(printer.mcu.mcu_build_versions)}
```
