
# 🏠 Smart Home Automation System (Proteus Simulation)

A **Smart Home Automation System** implemented using the **STM32F103** microcontroller and simulated in **Proteus**.
The system monitors **temperature** and **light levels** and automatically controls devices such as a **fan** and **lamp** based on predefined thresholds.

It also supports **manual control via push buttons** and **serial monitoring via UART** using a **Virtual Terminal**.

---

⚠️ **Important Note**

The **humidity measurement feature is not implemented in this version**.

* Humidity value is fixed at **100**
* Humidity warning is **always active**
* No humidity sensor exists in the Proteus schematic

This part is planned for **future implementation**.

---

# 📑 Table of Contents

* [Features](#features)
* [Unimplemented Parts](#unimplemented-parts)
* [Hardware Components](#hardware-components)
* [Schematic Diagram](#schematic-diagram)
* [System Operation](#system-operation)
* [User Guide](#user-guide)
* [Software Requirements](#software-requirements)
* [How to Run the Simulation](#how-to-run-the-simulation)
* [Future Improvements](#future-improvements)

---

# ✨ Features

### 🌡️ Sensor Monitoring

* **Temperature sensor (LM35)** reading via **ADC Channel 1**
* **Light sensor (LDR)** reading via **ADC Channel 0**
* Sensor values converted to real-world units and transmitted via UART

### 🤖 Automatic Control

* **Fan control** when temperature exceeds **40°C**
* **Lamp control** when light level drops below **20**
* Fan runtime managed using **Timer TIM3**

### 🎮 Manual Control

Using push buttons:

* Switch between **Automatic Mode** and **Manual Mode**
* Manually toggle **Fan** and **Lamp**

### 🧾 Serial Monitoring

Sensor data is transmitted every **1 second** via **UART**:

```
L: <lux>, T: <temp>, H: 100
```

Displayed in the **Proteus Virtual Terminal**.

### ⚠️ Warning System

* Humidity warning LED activated
* Terminal message:

```
humidity warning!!!
```

*(Always active in current version)*

### ⏱️ Timers

* **TIM2**

  * Generates **1-second periodic interrupts**
  * Prescaler = 7999
  * Period = 999 (8 MHz clock)

* **TIM3**

  * Controls **fan ON duration** in automatic mode

### 💡 Status Indicators

* **PB1 LED** indicates the microcontroller is running (always ON)

---

# 🚧 Unimplemented Parts

### Humidity Sensor

Currently not implemented.

Effects:

* Humidity value fixed to **100**
* Warning LED **always ON**
* Terminal always prints **H: 100**

---

# 🔧 Hardware Components

Simulated components include:

### Microcontroller

* **STM32F103C8**

### Sensors

* **LM35 Temperature Sensor** → PA1 (ADC Channel 1)
* **LDR Light Sensor + LM358 Op-Amp** → PA0 (ADC Channel 0)

### Actuators

* **2 Relays**

  * Fan
  * Lamp

### Relay Driver Circuit

* **BC547 Transistors**
* **1N4001 Flyback Diodes**

### Push Buttons

| Pin  | Function                  |
| ---- | ------------------------- |
| PA5  | Switch Auto / Manual mode |
| PB3  | Manual Lamp Control       |
| PA13 | Manual Fan Control        |

### Indicator LEDs

| Pin | Purpose                        |
| --- | ------------------------------ |
| PA6 | Mode Indicator (Manual / Auto) |
| PB0 | Lamp Status                    |
| PA7 | Fan Status                     |
| PB2 | Humidity Warning               |
| PB1 | System Status                  |

### Communication

* **USART1 → Virtual Terminal**

---

# 🖥️ Schematic Diagram

The Proteus schematic includes:

* STM32F103C8 with crystal oscillator and reset circuit
* LDR sensor with **LM358 signal amplification**
* Relay driver circuits with **BC547**
* Push buttons with pull-up resistors
* Status LEDs
* Virtual Terminal for UART monitoring
  
<img width="1070" height="841" alt="Schematic" src="https://github.com/user-attachments/assets/e52893ad-7e38-49a9-925b-48d57a5279e4" />

---

# ⚙️ System Operation

1️⃣ After startup, **TIM2** starts generating **1-second interrupts**.

2️⃣ Every second:

* ADC reads **temperature** and **light sensors**
* Values are converted to real units

3️⃣ In **Automatic Mode (`selfControl = 1`)**

* If **Temperature ≥ 40°C**

  * Fan turns ON
  * **TIM3** starts countdown to turn it OFF

* If **Light < 20**

  * Lamp turns ON

4️⃣ Humidity warning is triggered (fixed value).

5️⃣ Data is sent via **UART** to the terminal.

---

# 👨‍💻 User Guide

### ▶ Run Simulation

1. Open the **Proteus project**
2. Click **Run**
3. Sensor data appears in the **Virtual Terminal every second**

---

### 🔄 Switch Mode

Press **PA5 button**

| Mode      | LED PA6 |
| --------- | ------- |
| Automatic | OFF     |
| Manual    | ON      |

---

### 🎮 Manual Control

Only works in **Manual Mode**

| Button | Function    |
| ------ | ----------- |
| PB3    | Toggle Lamp |
| PA13   | Toggle Fan  |

---

### ⚠ View Warnings

Every second:

```
humidity warning!!!
```

* Terminal message appears
* **PB2 LED** stays ON

---

### ⚙ Adjust Thresholds

Thresholds can be modified in:

```
HAL_TIM_PeriodElapsedCallback()
```

Variables:

```
Lux_Threshold
Temp_Threshold
Humid_Threshold
```

---

# 💻 Software Requirements

Required tools:

* **STM32CubeMX** → Peripheral configuration
* **IAR Embedded Workbench** → Compilation & debugging
* **Proteus 8+** → Circuit simulation

---

# ▶ How to Run the Simulation

1. Open the project in **IAR Embedded Workbench**
2. Review or modify `main.c`
3. **Build the project** to generate the `.hex` file
4. In **Proteus**

   * Double-click the **STM32**
   * Assign the generated **HEX file**
5. Click **Run**

---

# 🚀 Future Improvements

Planned enhancements:

* Implement **real humidity sensor**
  *(DHT11 / DHT22)*

* Add **ADC humidity channel** for analog sensors

* Improve **fan runtime accuracy**

* Allow **threshold configuration via UART**


---

## 🎓 Academic Context

Developed as part of a **Microprocessor course**

📅 Fall 2024

---
