# Electronics Assessment IF

This repository contains two KiCad-based electronics design projects for an IMU/ESP32 hardware assessment. The work is organized into two separate PCB design folders, each with schematic, PCB layout, bill-of-material related CSV data, and generated manufacturing outputs.

## Project Overview

The assessment includes:
- A PCB design centered around an ESP32-based implementation in the folder `esp32_project`
- A second board layout project in the folder `imu_design`
- Generated Gerber files for fabrication readiness
- Project metadata and design export files for documentation and manufacturing handoff

## Repository Structure

```text
.
├── README.md
├── esp32_project/
│   ├── esp32_project.kicad_pro
│   ├── esp32_project.kicad_sch
│   ├── esp32_project.kicad_pcb
│   ├── ESP_For_IMU.csv
│   ├── freerouting.dsn
│   ├── Gerber_files/
│   └── report.txt
└── imu_design/
    ├── imu_design.kicad_pro
    ├── imu_design.kicad_sch
    ├── imu_design.kicad_pcb
    ├── ESP_For_IMU.csv
    ├── gerber_files/
    └── imu_design.pdf
```

## What is Included

### 1. ESP32 project
The `esp32_project` folder contains the KiCad project files for an ESP32-oriented board design. It includes:
- Schematic and PCB layout files
- A CSV export related to the IMU/ESP32 interface
- A routing file for external autorouting workflow
- Gerber files ready for fabrication review
- A project report file

### 2. IMU design
The `imu_design` folder contains a second KiCad design focused on the IMU-related hardware layout. It includes:
- Schematic and PCB layout files
- A CSV export associated with the IMU design data
- Gerber output files for manufacturing
- A PDF export of the PCB design

## Design Files

The projects use standard KiCad file formats:
- `.kicad_pro` for project configuration
- `.kicad_sch` for schematic design
- `.kicad_pcb` for PCB layout
- `.csv` for component or BOM-related data
- Gerber files for manufacturing output

## How to Open the Designs

1. Install KiCad on your system.
2. Open the desired project folder.
3. Open the `.kicad_pro` file to load the full project in KiCad.
4. Review the schematic, PCB layout, and generated Gerber outputs.

## Notes

- The project folders already contain generated manufacturing outputs, making them suitable for review or fabrication preparation.
- The repository is intended for hardware design assessment documentation and version control of the PCB work.
- The `report.txt` file in the ESP32 project folder can be expanded with design notes, test results, or manufacturing comments as needed.

## Future Improvements

Possible next steps for the project include:
- Adding a formal BOM file
- Documenting component references and footprints
- Including assembly notes and fabrication instructions
- Adding version history and revision tracking

## Author

This repository was prepared as part of the electronics assessment for the IF hardware/mechatronics take-home task.
