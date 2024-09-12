# Klipper Pressure Advance Macro

This repository contains a Klipper macro for automatically setting the Pressure Advance value based on the provided parameters.


## Macro

Add the following macro to your `printer.cfg` file:

```
# BOWDEN_LENGTH in centimeters
# LAYER_HEIGHT and NOZZLE_SIZE in millimeters
# PRINT_SPEED in millimeters per second
[gcode_macro CALCULATE_PA]
gcode:
    {% set material = params.MATERIAL|default("PLA") %}
    {% set bowden_length_cm = params.BOWDEN_LENGTH|default(31)|float %}
    {% set bowden_length_dm = bowden_length_cm / 10 %}  # Convert cm to decimeters for consistency
    {% set layer_height = params.LAYER_HEIGHT|default(0.2)|float %}
    {% set nozzle_size = params.NOZZLE_SIZE|default(0.4)|float %}
    {% set line_width = params.LINE_WIDTH|default(nozzle_size * 1.2)|float %}
    {% set print_speed = params.PRINT_SPEED|default(60)|float %}
    {% set filament_diameter = params.FILAMENT_DIAMETER|default(1.75)|float %}
    {% set filament_area = 3.1416 * (filament_diameter / 2) ** 2 %}
    {% set flow_rate = line_width * layer_height * print_speed %}
    {% set material_constant = {
        'PLA': 85,
        'PETG': 100,
        'ABS': 95,
        'TPU': 140,
        'NYLON': 120,
        'ASA': 100,
        'PVB': 85,
        'PA': 120,
        'FLEX': 140
    }[material] %}
    {% set pressure_advance = (flow_rate * bowden_length_dm) / (material_constant * filament_area) %}
    SET_PRESSURE_ADVANCE ADVANCE={pressure_advance}
```

## Usage

To use the macro in your G-Code files, add the following line in your PrusaSlicer Start GCode before the print starts:

```
CALCULATE_PA BOWDEN_LENGTH=6 MATERIAL=[filament_type] LAYER_HEIGHT=[layer_height] NOZZLE_SIZE=[nozzle_diameter] PRINT_SPEED=[perimeter_speed] FILAMENT_DIAMETER=[filament_diameter] LINE_WIDTH=[line_width]
```

This macro calculates the Pressure Advance value based on the provided parameters and sets it for the extruder. Note that the material constants in the macro definition are only meant as starting values. You can adjust these values to achieve the best performance for your specific material and printer.

## License

This project is licensed under the MIT License. See MIT LICENSE for more information.
