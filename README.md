# PWM-Based-DC-Motor-Speed-Controller-with-RPM-Display-and-Safety-Indication-System


## 🎯 Project Overview

RevAlert simulates a real-world industrial motor speed controller.
Turn a potentiometer → motor speeds up → LCD shows RPM live → 
LEDs glow by zone → buzzer warns at max speed.

---

## 🧰 Components Required

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

## 🔌 Pin Connections

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

## 🟢🟡🔴 Speed Zones

| Zone    | RPM Range | LED    |
|---------|-----------|--------|
| Safe    | 0 – 30    | Green  |
| Caution | 31 – 80   | Yellow |
| Danger  | 81 – 300  | Red    |
| MAX     | 282+      | Red + Buzzer ON |

---

## ⚙️ How It Works

1. Potentiometer rotated → ADC reads value (0–1023) on PF0
2. ADC mapped to PWM duty cycle (0–255) on Timer1 OC1B → PB6
3. L298N drives DC motor speed via ENA pin
4. RPM = PWM × 300 / 255
5. LCD displays Speed and RPM live
6. LED lights up based on speed zone
7. Buzzer turns ON when PWM ≥ 240 (~282+ RPM)

---

## 💻 Tools Used

- Language : C (AVR-GCC)
- Simulation : Proteus 8
- Board : ATmega2560 (Arduino Mega 2560)


---

## 👤 Author

- Name    : Chaudhary Vivek Rajeshbhai 
- College : Pandit Deendayal Energy University 
- Branch  : ICT
- Year    : 2026
