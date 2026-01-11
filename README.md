# Science Olympiad 2022: Detector Building (Sensor Code)

This repository contains the C++/Arduino firmware for the **Detector Building (Division B/C)** device for the 2022 Science Olympiad season.

The device is designed to measure input signals from a voltage divider probe, calculate the corresponding environmental variable, and provide visual feedback via a 3-LED system based on specific thresholds.

## 📖 Competition Context (2022 Rules)
* **Event:** Detector Building
* **Objective:** Build a durable probe to measure voltage and NaCl concentration (0-5000 ppm) in water samples.
* **Output Requirements:**
    * Display voltage and calculated PPM.
    * Utilize a 3-Zone LED system (Blue, Green, Red) to indicate concentration ranges.

## ⚙️ Hardware Specifications

The code is designed for an Arduino-compatible microcontroller (e.g., TI-Innovator, Arduino Uno/Nano).

### Pinout Configuration
| Component | Pin | Mode | Description |
| :--- | :--- | :--- | :--- |
| **Probe Input** | `A0` | `INPUT` | Voltage divider analog reading |
| **Blue LED** | `13` | `OUTPUT` | Low Range Indicator |
| **Green LED** | `12` | `OUTPUT` | Target/Mid Range Indicator |
| **Red LED** | `11` | `OUTPUT` | High Range Indicator |

### Circuit Design
* **Sensor Probe:** Configured as a voltage divider with a 10kΩ (`R1`) resistor.
* **LEDs:** Standard LEDs connected to digital pins with appropriate current-limiting resistors (220Ω-330Ω).

## 🧮 Code Logic & Mathematics

The firmware performs the following operations in the `loop()`:

1.  **Data Acquisition:** Reads the raw analog voltage (`Vo`) from the probe at Pin A0.
2.  **Resistance Calculation:** Converts raw voltage to resistance (`R2`) based on the known resistor `R1` (10,000Ω).
3.  **Steinhart-Hart Equation:** Currently utilizes the Steinhart-Hart coefficients to linearize the resistance curve.
    $$T = \frac{1}{A + B\ln(R) + C(\ln(R))^3}$$
4.  **Zone Logic:** Compares the calculated value against defined thresholds (`ppm1`, `ppm2`) to toggle LEDs.

### Calibration Constants
The following coefficients are currently set in the code:
```cpp
float c1 = 1.009249522e-03; // Coefficient A
float c2 = 2.378405444e-04; // Coefficient B
float c3 = 2.019202697e-07; // Coefficient C
