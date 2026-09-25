# Real-Time Effluent Quality Monitoring via Hybrid DWSIM Simulation and Machine Learning Soft Sensors

An industrial AI-IoT soft sensor platform for real-time monitoring of Effluent Treatment Plants (ETP). Built as an industry-linked Chemical Engineering project for the **Malanpur Industrial Area (Bhind/Gwalior, M.P.)**.

---

## 📌 Problem Statement
Traditional analytical laboratory methods for measuring organic pollutant loads have severe operational delays:
* **BOD₅ (5-day Biochemical Oxygen Demand):** Takes **120 hours** of incubation, preventing real-time control.
* **COD (Chemical Oxygen Demand):** Standard reflux digestion takes **2–3 hours** using hazardous chemical reagents ($K_2Cr_2O_7$ and concentrated $H_2SO_4$).

---

## 💡 Solution Architecture
A 3-tier hybrid cyber-physical-digital twin system:
1. **Physical Sensor Node (Arduino Uno):** Low-cost optical turbidity, analog pH, and DS18B20 digital temperature sensors read water parameters in real time.
2. **Process Digital Twin (DWSIM):** Rigorous chemical process flowsheet modeling biological aeration (CSTR) and secondary clarifier mass balances to generate synthetic boundary priors.
3. **ML Soft Sensor (Python):** Random Forest and Ridge Regression models predict hard-to-measure BOD₅ and COD in under 2 seconds.

---

## ⚙️ Hardware Node Specification

| Sensor / Component | Pin on Arduino | Measurement Range | Purpose |
|---|---|---|---|
| **Analog pH Probe** | A0 | 0 – 14 pH | Bio-kinetic enzyme suitability |
| **Optical Turbidity Sensor** | A1 | 0 – 3000 NTU | Particulate organic load indicator |
| **DS18B20 Temp Probe** | D2 (4.7kΩ pull-up) | -55°C to +125°C | Arrhenius biological reaction rate |
| **Microcontroller** | USB to PC | ATmega328P | 10-sample rolling average filter |

**Hardware Budget:** ₹2,150 (within ₹3,000 budget)

---

## 🏭 Partner Industry & Baseline Parameters
* **Target Industry:** Dairy Processing (Sterling Agro Industries Ltd. / SM Milkose Ltd., Malanpur).
* **Rationale:** High biodegradability (BOD/COD ratio 0.50–0.65), absence of toxic biocides, and stable biological sludge.

### Target Compliance (CPCB MINAS Standards):
* **pH:** 6.5 – 8.5
* **Turbidity:** < 50 NTU
* **Total Suspended Solids (TSS):** < 100 mg/L
* **Predicted COD:** < 250 mg/L
* **Predicted BOD₅:** < 30 mg/L

---

## 🔬 DWSIM Process Simulation Setup
* **Thermodynamic Package:** NRTL / Raoult's Law (Aqueous).
* **Components:** Water, Glucose/Lactose, Lactic Acid, Oxygen, Nitrogen, Carbon Dioxide.
* **Unit Operations:**
  * Raw Wastewater Feed ($35\text{ m}^3/\text{h}$)
  * Air Ingestion Blower ($DO \ge 2.0\text{ mg/L}$)
  * Biological Aeration Tank (CSTR with Monod Kinetics)
  * Secondary Clarifier (Solid-Liquid Separation)
  * Sludge Recycle Loop (70% Return Activated Sludge)

---

## 🚀 Setup & Execution

### 1. Arduino Sensor Node
Flash the C++ firmware to Arduino Uno via Arduino IDE to stream real-time JSON sensor readings over USB Serial at 9600 baud:
```json
{"pH": 7.34, "turbidity_ntu": 24.8, "temp_c": 27.2}
