# Internal System V0.0

This document is a draft overview of the current system hardware connections and basic electrical requirements.

## LiDAR

### Hardware connections
- LiDAR connects to the interface box.
- **Interface box**
  - LAN cable to the computer
  - Barrel jack to the power supply
  - Multi-purpose I/O to GNSS TX
  - Sync pulse to PPS

### Internal IMU
- 3-axis gyro
- 3-axis accelerometer

### Electrical requirements
- Operating voltage: 12 V DC or 24 V DC
- Peak power: 22 W
- Typical power: 10–20 W
- Use a power supply rated at 30 W or higher

## GNSS

### Hardware connections
- TXD → Multi-purpose I/O on the interface box
- RXD → MCU serial input
- TXD2 → MCU UART2 RX (secondary GNSS serial path)
- RXD2 → MCU UART2 TX (secondary GNSS serial path)
- Time pulse / PPS → Sync pulse input

### Connection notes
- Use the GNSS UART port for normal position/time data.
- Use TXD2/RXD2 only if you need the second serial interface on the GNSS module.
- Keep the PPS line as a dedicated timing input because it is used for synchronization.

### Electrical requirements
- Operating voltage: 5 V DC

## MCU (Arduino / Blue Pill)

The current schematic uses an STM32F103C8T6-style Blue Pill, so this is the primary target controller for the system.

### Arduino

- Connect GNSS TXD to the Arduino RX pin and GNSS RXD to the Arduino TX pin.
- Connect PPS to a digital interrupt-capable input pin for time pulse capture.
- Power the GNSS module from 5 V DC and the MCU from its normal operating voltage.

### Blue Pill
- The Blue Pill is the recommended MCU for this design.
- Connect GNSS TXD to the MCU UART RX pin and GNSS RXD to the MCU UART TX pin.
- Use a dedicated PPS input pin (for example, an EXTI-capable GPIO) for synchronization.
- Provide 3.3 V logic power to the STM32 side and keep GND common between all modules.
- This board is suitable for serial communication, PPS timing capture, and LiDAR/GNSS coordination.
