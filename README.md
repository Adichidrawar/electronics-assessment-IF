# Electronics Assessment IF

This repository contains two KiCad-based electronics design projects for an IMU and ESP32-based hardware assessment. The work is organized into two PCB design folders with schematic, layout, CSV data, and manufacturing output files.

## Project Overview

The assessment includes:
- An ESP32-oriented board design in the esp32_project folder
- A second IMU-focused board design in the imu_design folder
- Generated Gerber files for fabrication review
- Project metadata and design exports for documentation and handoff

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

## Architecture and block diagram

A simple view of the intended system is:

```text
IMU sensor board ---> 300 mm cable ---> ESP32 host board ---> USB/serial or wireless link
      |                                |
      |                                +--> debug UART / status LEDs
      +--> power and ground return ---+
```

The architecture is intentionally simple:
1. The IMU board collects motion data from the sensor.
2. That data is transferred to the ESP32 board over a short, controlled digital link.
3. The ESP32 board handles communication, buffering, and any downstream logging or wireless transmission.
4. The cable carries only the signals needed for reliable timing and low-noise transfer: power, ground, clock, chip-select, data in/out, and optionally an interrupt line.

## What crosses the 300 mm cable

I would keep the cable as a compact, low-noise digital link rather than trying to carry analog signals over a long run. In practice, the following would cross the 300 mm cable:
- 3.3 V power and ground
- SPI signals: SCLK, MOSI, MISO, CS
- Optionally an interrupt or data-ready line from the IMU to the host

Using SPI is the most practical choice because it is synchronous, relatively simple to debug, and is well suited to IMU register reads and burst transfers. I would not try to carry analog outputs or high-speed clocks over this cable unless the wiring is tightly controlled and shielded.

For a 300 mm cable, I would trust roughly 4 MHz SPI as a sensible starting point. 8 MHz is possible with good layout, controlled impedance, and proper grounding, but it is a stretch for a generic cable assembly. I would prefer 2 to 4 MHz for the first prototype and only raise it after validating signal integrity.

## How I would timestamp IMU samples

I would timestamp samples on the ESP32 side using a free-running hardware timer with microsecond resolution. The approach would be:
- Capture the current timer value immediately after the IMU interrupt or after the SPI transaction completes.
- Store that timestamp together with the received sample data.
- If possible, use the IMU data-ready interrupt to latch the time at the source rather than relying on a later software poll.

For a first-pass implementation, I would expect timing error on the order of tens of microseconds, roughly 10 to 50 microseconds, depending on SPI latency, interrupt handling, and scheduler jitter. That is usually good enough for slow-motion IMU logging, but it would not be suitable for very high-rate control loops without more careful real-time design.

## Component choices

The main parts would be:
- ESP32-WROOM-32 or similar ESP32 module: the main compute and communications hub, with enough GPIO and SPI capability for the IMU link.
- A 6-axis or 9-axis IMU such as ICM-42688 or MPU-6050: provides the motion sensing and a straightforward digital interface.
- A 3.3 V low-dropout regulator: keeps the logic rail stable and clean for the sensor and the ESP32.
- Decoupling capacitors and a small reset/boot network: these are essential for reliable startup and noise immunity.
- ESD and filtering parts on the cable side: helpful for protecting the board and reducing transient noise when the cable is moved or connected.

## Rough power budget

A realistic rough budget for a simple prototype is:
- ESP32 active: about 200 to 300 mA at 3.3 V
- IMU sensor: about 1 to 3 mA
- LDO and small support circuitry: a few milliamps

That gives a total active load of roughly 0.3 to 0.8 W, depending on Wi-Fi or Bluetooth activity. For a safe design margin, I would plan for a 3.3 V supply that can comfortably deliver at least 500 mA and preferably 1 A if the ESP32 is doing wireless work.

## What is most likely to be wrong

The three biggest risks on first bring-up are:
1. Cable or connector integrity: a poor ground return, bad pinout, or marginal connector contact can make SPI look flaky.
2. Power integrity: the 3.3 V rail may dip during startup or wireless bursts, causing resets or failed sensor reads.
3. Timing and signal quality: the IMU may fail to read correctly if the SPI clock is too fast or if the bus is not cleanly routed.

When the boards arrive, I would test in this order:
- Check the 3.3 V rail at the IMU and ESP32 pins under load.
- Verify continuity and ground return between the boards.
- Perform a simple SPI register read from the IMU using a known register value and confirm the response.
- Only then move to higher-rate logging or wireless transmit tests.

## What was not finished

The current design is a good starting point, but it is not yet a fully finished hardware package. The main unfinished items are:
- A formal BOM with exact part numbers and package sizes
- More careful connector and cable pinout documentation
- A bring-up checklist and test points for fast debugging

With another 3 hours, I would focus on:
- Completing the BOM and part references
- Adding test points for power, reset, SPI, and the IMU interrupt
- Writing a simple boot-and-read procedure so the first board bring-up is fast and repeatable

## How to open the designs

1. Install KiCad on your system.
2. Open the desired project folder.
3. Open the .kicad_pro file to load the full project in KiCad.
4. Review the schematic, PCB layout, and generated Gerber outputs.

## Notes

- The project folders already contain generated manufacturing outputs, making them suitable for review or fabrication preparation.
- The repository is intended for hardware design assessment documentation and version control of the PCB work.
- The report.txt file in the ESP32 project folder can be expanded with design notes, test results, or manufacturing comments as needed.

## Author

This repository was prepared as part of the electronics assessment for the IF hardware/mechatronics take-home task.
