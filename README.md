# SmartSort - Automated Waste Sorting System

SmartSort is an Arduino UNO-based edge device designed to perform real-time binary classification of waste at the moment of disposal. By automatically separating wet organic waste from dry recyclables, it prevents cross-contamination and preserves the recyclability of paper and cardboard materials.

---

## System Overview

* **Microcontroller:** Arduino UNO
* **Detection:** HC-SR04 Ultrasonic Sensor (Proximity detection < 20 cm)
* **Classification:** YL-69 Moisture Sensor (Analog reading; threshold set at 900)
* **Actuation:** SG90 Servo Motor (160° for Wet, 20° for Dry, 90° Center)
* **Display:** 16x2 I2C LCD Screen

---

## Circuit Connections

| Component | Component Pin | Arduino Pin |
| :--- | :--- | :--- |
| **HC-SR04 Ultrasonic** | Trig | Digital 9 |
| **HC-SR04 Ultrasonic** | Echo | Digital 8 |
| **YL-69 Moisture** | AO | Analog A0 |
| **SG90 Servo** | Signal (Orange) | Digital 12 |
| **I2C LCD** | SDA / SCL | A4 / A5 |

---

## Operating Logic

1. **Idle State:** System displays `Ready for Waste` on the I2C LCD screen.
2. **Proximity Trigger:** When an object comes within 20 cm, the system enters the placement phase.
3. **Sampling:** After a 5-second delay to position the item on the probe, the sensor samples moisture level on Pin A0.
4. **Classification & Sorting:**
   * **Moisture < 900:** Classified as **WET WASTE** $\rightarrow$ Servo swings to 160° (Left Bin).
   * **Moisture $\ge$ 900:** Classified as **DRY WASTE** $\rightarrow$ Servo swings to 20° (Right Bin).
5. **Reset:** The flap holds for 4 seconds, returns to the 90° center position, and updates the local item counter.
