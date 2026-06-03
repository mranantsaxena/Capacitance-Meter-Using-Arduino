# Capacitance-Meter-Using-Arduino

Tinkercad Simulation Link : https://www.tinkercad.com/things/bgTAE0WUXUk-capacitance-meter-?sharecode=E3aBqRws3P-2sWHZwpBqvM9aWMcpQypnoRMOQE42gGI

An Arduino-based Capacitance Meter simulated in Tinkercad. It measures capacitance by calculating the RC time constant during charging (10kΩ resistor) and safely resets via a 1kΩ discharge resistor. Real-time results are output to the Serial Monitor in microfarads (µF). Ideal for fundamental electronics and embedded systems projects.

## ⚡ How It Works (The Math)

This project relies on the physics of an RC (Resistor-Capacitor) circuit. When a capacitor charges through a resistor, the voltage across it increases over time according to the equation:

Vc(t) = Vs * (1 - e^(-t/RC))

The time it takes for the capacitor to reach approximately **63.2%** of its maximum voltage (Vs) is defined as exactly one **Time Constant (τ)**. 

τ = R * C

By measuring how long it takes the capacitor to reach this 63.2% voltage threshold (using the Arduino's internal timer), and knowing our fixed resistor value, we can rearrange the formula to solve for Capacitance (C):

C = τ / R

## 🛠️ Components & Wiring

This simulation uses standard components available in Tinkercad:
* 1x Arduino Uno R3
* 1x Breadboard
* 1x Capacitor (Tested with 10 µF)
* 1x 10kΩ Resistor (Charging)
* 1x 1kΩ Resistor (Discharging)

### Pin Configuration
| Component / Wire | Arduino Pin | Description |
| :--- | :--- | :--- |
| **Orange Wire** | `A0` | Analog input to measure the voltage across the capacitor. |
| **Red Wire** | `D8` | Outputs 5V through the 10kΩ resistor to charge the capacitor. |
| **Black Wire** | `D9` | Connects to ground through the 1kΩ resistor to discharge the capacitor. |
| **Green Wire** | `GND` | Common ground for the capacitor's negative terminal. |

## 🚀 Running the Simulation in Tinkercad

1. Create a new circuit in Tinkercad and wire the components according to the table above.
2. Open the **Code** panel and switch the view from "Blocks" to **"Text"**.
3. Paste the contents of your Arduino code into the editor.
4. Click the **Serial Monitor** tab at the bottom of the code panel.
5. Click **Start Simulation**.
6. Watch the Serial Monitor print the charge time in microseconds and the calculated capacitance in µF!

## ⚙️ Code Logic Overview

1.  **Discharge Phase:** Digital Pin 9 is driven `LOW` to safely drain any remaining charge from the capacitor through the 1kΩ resistor.
2.  **Charge Phase:** Digital Pin 8 is driven `HIGH` (5V). The `micros()` function records the start time.
3.  **Measurement Phase:** The Arduino continuously reads Analog Pin `A0` until it hits `648` (which is ~63.2% of the ADC's 1023 max resolution).
4.  **Calculation:** The time elapsed is divided by the 10,000Ω resistance to output the capacitance directly in microfarads.
