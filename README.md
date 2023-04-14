# Klipper Pressure Advance Macro

This repository contains a Klipper macro for automatically setting the Pressure Advance value based on the provided parameters.

## Macro

Add the following macro to your `printer.cfg` file:

```
[gcode_macro CALCULATE_PA]
default_parameter_MATERIAL: PLA
default_parameter_BOWDEN_LENGTH: 31
default_parameter_LAYER_HEIGHT: 0.2
default_parameter_NOZZLE_SIZE: 0.4
default_parameter_PRINT_SPEED: 180
gcode:
    {% set material_constant = {'PLA': 100, 'PETG': 120, 'ABS': 110, 'Nylon': 130, 'TPU': 150, 'PVB': 100}[MATERIAL] %}
    {% set vfr = NOZZLE_SIZE|float * LAYER_HEIGHT|float * PRINT_SPEED|float %}
    {% set pressure_advance = (vfr * BOWDEN_LENGTH|float) / material_constant %}
    SET_PRESSURE_ADVANCE ADVANCE={pressure_advance}
```

## Usage

To use the macro in your G-Code files, add the following line before the print starts:

```
CALCULATE_PA MATERIAL=PLA BOWDEN_LENGTH=31 LAYER_HEIGHT=0.2 NOZZLE_SIZE=0.4 PRINT_SPEED=180
```

This macro calculates the Pressure Advance value based on the provided parameters and sets it for the extruder. Note that the material constants in the macro definition are only meant as starting values. You can adjust these values to achieve the best performance for your specific material and printer.

## Usage

PrusaSlicer Start G-Code Integration

```
; Call the Pressure Advance macro
CALCULATE_PA MATERIAL=PLA BOWDEN_LENGTH=20 LAYER_HEIGHT=[layer_height] NOZZLE_SIZE=[nozzle_diameter] PRINT_SPEED=[perimeter_speed]
```

## License

This project is licensed under the MIT License. See [LICENSE](LICENSE) for more information.
