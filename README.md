# Klipper Pressure Advance Macro

This repository contains a Klipper macro for automatically setting the Pressure Advance value based on the provided parameters.


## Macro

Add the following macro to your `printer.cfg` file:

```
# BOWDEN_LENGTH in centimeters
# LAYER_HEIGHT and NOZZLE_SIZE in millimeters
# PRINT_SPEED in millimeters per secound
[gcode_macro CALCULATE_PA]
gcode:
    {% set material = params.MATERIAL|default("PLA") %}
    {% set bowden_length = (params.BOWDEN_LENGTH|default(31)|float) / 10 %}
    {% set layer_height = params.LAYER_HEIGHT|default(0.2)|float %}
    {% set nozzle_size = params.NOZZLE_SIZE|default(0.4)|float %}
    {% set print_speed = params.PRINT_SPEED|default(180)|float %}
    {% set material_constant = {'PLA': 100, 'PET': 120, 'PETG': 120, 'ABS': 110, 'Nylon': 130, 'TPU': 150, 'PVB': 100}[material] %}
    {% set vfr = nozzle_size * layer_height * print_speed %}
    {% set pressure_advance = (vfr * bowden_length) / material_constant %}
    SET_PRESSURE_ADVANCE ADVANCE={pressure_advance}
```

## Usage

To use the macro in your G-Code files, add the following line in your PrusaSlicer Start GCode before the print starts:

```
CALCULATE_PA BOWDEN_LENGTH=6  MATERIAL=[filament_type] LAYER_HEIGHT=[layer_height] NOZZLE_SIZE=[nozzle_diameter] PRINT_SPEED=[perimeter_speed]
```

This macro calculates the Pressure Advance value based on the provided parameters and sets it for the extruder. Note that the material constants in the macro definition are only meant as starting values. You can adjust these values to achieve the best performance for your specific material and printer.

## License

This project is licensed under the MIT License. See MIT LICENSE for more information.
