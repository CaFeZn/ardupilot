# DM‑FC01 Flight Controller

The DM‑FC01 is a flight controller based on the STM32H743 processor, designed for high-performance UAV applications.

## Features

### Processor
- STM32H743 32-bit processor
- 480MHz
- 2MB Flash
- 1MB RAM

### Sensors
- Primary IMU: Bosch BMI088 (SPI)
- Secondary IMU: Invensense ICM45686 (SPI)
- Barometer: SPL06 (I2C address 0x77)
- No built-in magnetometer (external compass recommended)

### Power
- 5.5V - 25.2V (2S - 6S) input
- Battery voltage monitoring via PC4 (ADC1_INP4_BATT_V)
- Battery current monitoring via PC5 (ADC2_INP8_BATT_I)
- Dual IMU heaters (PD14 for BMI088, PD15 for ICM45686)

### Interfaces
- **USB**: OTG1 (PA11/PA12) with VBUS detection on PA15
- **CAN1**: PD0 (RX), PD1 (TX)
- **GPS1**: USART1 (PA9/PA10)
- **TELEM1**: USART2 (PD5/PD6)
- **DJI O3**: USART3 (PD8/PD9)
- **TELEM2**: UART8 (PE1/PE0)
- **RC_INPUT**: UART5 (PB12/PB13) with SBUS inverter on PE15
- **ESC Telemetry**: UART7 RX (PE7)
- **TELEM3**: UART4 (PB8/PB9)
- **TELEM4**: USART6 (PC6/PC7)
- **I2C1**: PB6/PB7 (barometer)
- **I2C4**: PD12/PD13 (external connector)
- **SPI1**: IMU0 (BMI088)
- **SPI2**: External flash (W25Q128JVSQ 16MB)
- **SPI4**: IMU1 (ICM45686)
- **SD Card**: SDMMC1 (PC8-PC12, PD2)
- **PWM Outputs**: 8 channels with bidirectional DShot support

### Storage
- 16MB Winbond W25Q128JVSQ SPI flash for data logging
- microSD card support

### LEDs
- Red LED: PE12
- Green LED: PD11
- Blue LED: PB15

## Pinout

**Note**: All connectors use SH1.0 connectors unless otherwise specified.

### Connector Layout

#### GPS1 (6-pin SH1.0)
| Pin | Signal Name | Voltage | STM32 Pin |
|-----|-------------|---------|-----------|
| 1   | GND         | GND     | -         |
| 2   | 5.0V        | 5.0V    | -         |
| 3   | USART1_TX   | 3.3V    | PA9       |
| 4   | USART1_RX   | 3.3V    | PA10      |
| 5   | I2C4_SCL    | 3.3V    | PD12      |
| 6   | I2C4_SDA    | 3.3V    | PD13      |

#### TELEM1 (4-pin SH1.0)
| Pin | Signal Name | Voltage | STM32 Pin |
|-----|-------------|---------|-----------|
| 1   | GND         | GND     | -         |
| 2   | 5.0V        | 5.0V    | -         |
| 3   | USART2_TX   | 3.3V    | PD5       |
| 4   | USART2_RX   | 3.3V    | PD6       |

**Note**: USART2 does not have hardware flow control on this board.

#### RC_INPUT (4-pin SH1.0)
| Pin | Signal Name | Voltage | STM32 Pin |
|-----|-------------|---------|-----------|
| 1   | GND         | GND     | -         |
| 2   | 5.0V        | 5.0V    | -         |
| 3   | UART5_TX    | 3.3V    | PB13      |
| 4   | UART5_RX    | 3.3V    | PB12      |

**SBUS Inverter**: PE15 (active low)

#### CAN1 (4-pin SH1.0)
| Pin | Signal Name |   Voltage   | STM32 Pin |
|-----|-------------|-------------|-----------|
| 1   | GND         | GND         | -         |
| 2   | VBAT        | **Battery** | -         |
| 3   | CAN1_H      |             | -         |
| 4   | CAN1_L      |             | -         |

#### PWM Outputs (8-pin SH1.0)
| Pin | Signal Name |   Voltage   | STM32 Pin | Function |
|-----|-------------|-------------|-----------|----------|
| 1   | BATY_I      | 3.3V        | PC5       | Battery current |
| 2   | UART7_RX    | 3.3V        | PE7       | ESC telemetry (receive only) |
| 3   | PWM1        | 3.3V        | PE9       | TIM1_CH1 |
| 4   | PWM2        | 3.3V        | PE11      | TIM1_CH2 |
| 5   | PWM3        | 3.3V        | PE13      | TIM1_CH3 |
| 6   | PWM4        | 3.3V        | PE14      | TIM1_CH4 |
| 7   | Battery     | **Battery** | -         | Battery input |
| 8   | GND         | GND         | -         | Ground   |

**Note**: UART7 is receive-only for ESC telemetry.

#### PWM Outputs (5-pin SH1.0)
| Pin | Signal Name | Voltage | STM32 Pin | Function |
|-----|-------------|---------|-----------|----------|
| 1   | GND         | GND     | -         | Ground   |
| 2   | PWM8        | 3.3V    | PB1       | TIM3_CH4 |
| 3   | PWM7        | 3.3V    | PB0       | TIM3_CH3 |
| 4   | PWM6        | 3.3V    | PB10      | TIM2_CH3 |
| 5   | PWM5        | 3.3V    | PB11      | TIM2_CH4 |

#### AuxGPIO (6-pin SH1.0)
| Pin | Signal Name | Voltage | STM32 Pin |
|-----|-------------|---------|-----------|
| 1   | AUX3_PA4    | 3.3V    | PA4       |
| 2   | AUX2_PC1    | 3.3V    | PC1       |
| 3   | AUX1_PC0    | 3.3V    | PC0       |
| 4   | SPI3_MOSI   | 3.3V    | PB2       |
| 5   | SPI3_MISO   | 3.3V    | PB4       |
| 6   | SPI3_SCK    | 3.3V    | PB3       |

**⚠️ Attention:**
- These 6 pins are directly connected to MCU I/Os with no isolation components
- Applying voltages above 3.3V will directly damage the MCU
- Always verify the voltage level of connected devices to avoid damaging the flight controller

#### DJI O3 Air Unit (6-pin SH1.0)
| Pin | Signal Name | Voltage | STM32 Pin | Function |
|-----|-------------|---------|-----------|----------|
| 1   | 8.9V        | 8.9V    | -         | DJI O3 power output |
| 2   | GND         | GND     | -         | Power ground |
| 3   | USART3_TX   | 3.3V    | PD8       | MAVLink/OSD telemetry TX (FC -> O3 RX) |
| 4   | USART3_RX   | 3.3V    | PD9       | MAVLink telemetry RX (FC <- O3 TX) |
| 5   | GND         | GND     | -         | Signal ground |
| 6   | SBUS        | 3.3V    | PB12      | RC input from DJI receiver (UART5_RX / SERIAL5) |

#### TELEM2 (4-pin SH1.0)
| Pin | Signal Name | Voltage | STM32 Pin |
|-----|-------------|---------|-----------|
| 1   | GND         | GND     | -         |
| 2   | 5.0V        | 5.0V    | -         |
| 3   | UART8_TX    | 3.3V    | PE1       |
| 4   | UART8_RX    | 3.3V    | PE0       |

#### TELEM3 (3-pin test point)
| Pin | Signal Name | Voltage | STM32 Pin |
|-----|-------------|---------|-----------|
| 1   | UART4_RX    | 3.3V    | PB8       |
| 2   | GND         | GND     | -         |
| 3   | UART4_TX    | 3.3V    | PB9       |

#### TELEM4 (3-pin test point)
| Pin | Signal Name | Voltage | STM32 Pin |
|-----|-------------|---------|-----------|
| 1   | USART6_RX   | 3.3V    | PC7       |
| 2   | GND         | GND     | -         |
| 3   | USART6_TX   | 3.3V    | PC6       |

## UART Mapping

| Name     | Function           | Hardware | Default Protocol |
|----------|--------------------|----------|------------------|
| SERIAL0  | USB                | OTG1     | MAVLink          |
| SERIAL1  | TELEM1             | USART2   | MAVLink          |
| SERIAL2  | TELEM2             | UART8    | MAVLink          |
| SERIAL3  | GPS1               | USART1   | GPS              |
| SERIAL4  | DJI O3             | USART3   | MSP              |
| SERIAL5  | RC_INPUT           | UART5    | RCIN             |
| SERIAL6  | ESC Telemetry      | UART7    | ESC Telemetry    |
| SERIAL7  | TELEM3             | UART4    | -                |
| SERIAL8  | TELEM4             | USART6   | -                |

## RC Input

RC input is configured on UART5 (PB12/PB13). It supports all RC protocols including:
- SBUS (with inverter on PE15)
- DSM
- SRXL
- CRSF (requires both RX and TX)
- FPort (requires TX and SERIAL5_OPTIONS=7)

## Battery Monitoring

The board has analog battery monitoring:
- Voltage: PC4 (ADC1_INP4_BATT_V)
- Current: PC5 (ADC2_INP8_BATT_I)

Default parameters:
- BATT_MONITOR: 4 (Analog Voltage and Current)
- BATT_VOLT_PIN: 4
- BATT_CURR_PIN: 8
- BATT_VOLT_MULT: 10.2 (needs calibration)
- BATT_AMP_PERVOLT: 20.4 (needs calibration)

**Note**: Scaling factors need to be calibrated based on actual hardware measurements.

## Compass

No built-in compass. Use an external I2C compass connected to I2C1 or I2C4.

## Motor Output

All 8 PWM outputs support bidirectional DShot. Outputs are grouped as:
- Motors 1-4: Group1 (TIM1)
- Motors 5-6: Group2 (TIM2)
- Motors 7-8: Group3 (TIM3)

Channels within the same group must use the same output protocol (PWM or DShot).

## IMU Configuration

- Primary IMU: BMI088 on SPI1 (ROTATION_PITCH_180)
- Secondary IMU: ICM45686 on SPI4 (ROTATION_PITCH_180)

Both IMUs have independent heater control:
- BMI088 heater: PD14
- ICM45686 heater: PD15

## Loading Firmware

The board uses the ArduPilot bootloader. Firmware can be loaded via:
1. USB using `uploader.py`
2. SWD via the debug port (PA13/PA14)

## Building Firmware

```bash
./waf configure --board=DM-FC01
./waf copter  # or plane, rover, etc.
```

## Default Parameters

A `defaults.parm` file is provided in this directory with basic configuration:
- Battery monitoring enabled
- Serial protocols configured
- IMU heaters enabled with default temperature targets

## Board ID

- APJ_BOARD_ID: AP_HW_DM_FC01 (7140)
- Registered in `Tools/AP_Bootloader/board_types.txt`

## Notes

- SBUS inverter is on PE15 (different from NxtPX4v2's PD14)
- Dual IMU heaters allow independent temperature control
- External flash (W25Q128JVSQ) provides 16MB for data logging
- USB VBUS detection on PA15
