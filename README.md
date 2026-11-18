# RevAlert · PWM DC Motor Speed Controller

An ATmega2560 motor control demo in Proteus. A potentiometer sets the PWM duty cycle, a 16×2 LCD displays an estimated speed, and LEDs and a buzzer show threshold crossings.

[Watch the simulation](<Screen Recording 2026-04-22 190347.mp4>) · [Read the AVR C source](Code)

## How it works

Potentiometer → ADC0 → Timer1 PWM → L298N → DC motor

The display scales the PWM command to a nominal 0–300 RPM range using integer arithmetic. This is an open-loop estimate derived from the command. At PWM 240 or higher, the LCD displays `RPM: MAX!` and the buzzer sounds.

| PWM command | Indication |
| --- | --- |
| 0–25 | Green LED |
| 26–68 | Yellow LED |
| 69–239 | Red LED |
| 240–255 | Red LED and buzzer |

## Components

- ATmega2560 (Arduino Mega 2560)
- L298N Motor Driver Module
- DC Motor (5V or 12V)
- 16x2 LCD Display (LM016L)
- Potentiometer 10kΩ × 2
- LED Red × 1
- LED Green × 1
- LED Yellow × 1
- Active Buzzer 5V × 1
- 220Ω Resistors × 3
- Jumper Wires
- Breadboard

---

## Pin connections

### LCD (16x2)
| LCD Pin | Connect To           |
|---------|----------------------|
| VSS     | GND                  |
| VDD     | +5V                  |
| VEE     | Potentiometer middle |
| RS      | PD0                  |
| RW      | GND                  |
| E       | PD1                  |
| D4      | PD4                  |
| D5      | PD5                  |
| D6      | PD6                  |
| D7      | PD7                  |

### L298N Motor Driver
| L298N Pin | Connect To |
|-----------|------------|
| VS        | +5V        |
| VCC       | +5V        |
| IN1       | PB4        |
| IN2       | PB5        |
| ENA       | PB6 (PWM)  |
| OUT1/OUT2 | DC Motor   |
| GND       | GND        |

### Other Components
| Component     | Pin           |
|---------------|---------------|
| Potentiometer | PF0 (ADC)     |
| LED Green     | PB1           |
| LED Yellow    | PB2           |
| LED Red       | PB0           |
| Buzzer (+)    | PE3           |
| Buzzer (-)    | GND           |

---

## Development tools

AVR-GCC · ATmega2560 · Proteus 8
