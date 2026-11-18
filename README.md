# RevAlert | ATmega2560 PWM Motor Controller

RevAlert demonstrates an adjustable DC motor command, a 16×2 LCD speed estimate, and visual and audible indications in Proteus. The AVR firmware uses ADC channel 0 for input, Timer1 for PWM, and digital outputs for the LCD, indicators, and buzzer.

[Watch the Proteus demonstration](<Screen Recording 2026-04-22 190347.mp4>) · [Read the AVR C firmware](Code)

## Signal flow

```text
10 kΩ potentiometer ── ADC0 ── Scaled PWM command ── Timer1 OC1B ── L298N ── DC motor
                                      ├── 16×2 LCD
                                      └── Speed LEDs and buzzer
```

1. **Read the requested speed.** The ATmega2560 reads the voltage from the control potentiometer on PF0. The 10-bit ADC produces values from 0 to 1023.
2. **Scale the input.** The firmware calculates `PWM = floor(ADC × 255 / 1023)`. It writes the resulting command to `OCR1B`. Timer1 runs in 8-bit fast PWM mode and drives the motor enable through OC1B on PB6.
3. **Drive the motor.** The L298N receives the PWM signal at ENA. The firmware keeps IN1 high and IN2 low for one direction of rotation.
4. **Update the interface.** The first LCD row displays the PWM command. Below the alert threshold, the second row displays `floor(PWM × 300 / 255)` as a nominal RPM estimate. At a command of 240 or higher it shows `RPM: MAX!`.
5. **Update indications.** The firmware chooses one LED from the command range and activates the buzzer only in the top range. The main loop delays 300 ms after updating the outputs.

## Command ranges

| PWM command | Active LED | Buzzer | LCD second row |
| --- | --- | --- | --- |
| 0–25 | Green | Off | Estimated RPM |
| 26–68 | Yellow | Off | Estimated RPM |
| 69–239 | Red | Off | Estimated RPM |
| 240–255 | Red | On | `RPM: MAX!` |

The thresholds use the PWM command. The firmware computes a nominal speed estimate from a fixed 300 RPM scale and contains no encoder or tachometer input. Motor speed under load can differ from the displayed value. The buzzer and LEDs demonstrate threshold logic for this simulation.

## Components and pin mapping

The setup uses an ATmega2560, an L298N motor driver, a DC motor, a 16×2 LCD (LM016L in the simulation), a 5 V active buzzer, three LEDs with 220 Ω series resistors, and two 10 kΩ potentiometers for speed input and LCD contrast. The table uses AVR port labels.

| Module | Signal | Connection |
| --- | --- | --- |
| Speed potentiometer | Wiper | PF0 / ADC0 |
| L298N | IN1 / IN2 | PB4 / PB5 |
| L298N | ENA | PB6 / OC1B |
| L298N | OUT1 / OUT2 | DC motor terminals |
| L298N | VS / VCC / GND | +5 V / +5 V / common GND in the documented simulation |
| 16×2 LCD | RS / E | PD0 / PD1 |
| 16×2 LCD | D4–D7 | PD4–PD7 |
| 16×2 LCD | VSS / VDD / RW | GND / +5 V / GND |
| 16×2 LCD | Contrast / VEE | Wiper of the contrast potentiometer |
| LEDs | Green / yellow / red | PB1 / PB2 / PB0 |
| Buzzer | Control / return | PE3 / GND |

## Recreate or verify the simulation

The repository contains the AVR C source and a recording. To recreate the circuit in Proteus, build the source with AVR-GCC for the ATmega2560, set the build clock to match the simulator, and load the compiled firmware into the MCU model. Wire the simulation using the port labels above.

| Check | Expected observation |
| --- | --- |
| Power on | The LCD shows the startup message, then the current command and estimate. |
| Sweep the control potentiometer | The displayed PWM command increases from the low end toward 255. The driver enable signal changes duty cycle. |
| Cross PWM 25 → 26 and 68 → 69 | The LED changes from green to yellow, then from yellow to red. |
| Cross PWM 239 → 240 | The red LED stays on, the buzzer turns on, and the LCD changes to `RPM: MAX!`. |
| Return below PWM 240 | The buzzer turns off and the RPM estimate returns. |

## Repository contents

- [Code](Code) contains the ADC and Timer1 configuration, four-bit LCD routines, LED and buzzer logic, and the control loop.
- [Simulation recording](<Screen Recording 2026-04-22 190347.mp4>) captures the Proteus demonstration.
