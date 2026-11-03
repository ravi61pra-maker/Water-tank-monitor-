# Water-tank-monitor-

## Smart Water Tank Monitoring System Using Arduino

## 📌 Project Overview

The **Water Tank Monitor** is a smart water-level monitoring and pump control system developed as a college mini project for the Bachelor of Science in Computer Science (BSc CS).

The system uses an Arduino and an ultrasonic sensor to continuously monitor the water level inside a tank. The measured water level is displayed on a 16×2 I2C LCD and the Arduino Serial Monitor.

The system supports automatic and manual pump control. In automatic mode, the pump turns ON when the water level reaches the low-level threshold and turns OFF when the tank reaches the high-level threshold. A buzzer alert is activated when the water level reaches 85%.

The main objective of this project is to reduce water overflow, prevent water wastage, and make water tank monitoring easier.

---

## 🎯 Objectives

- To monitor the water level in a tank continuously.
- To display the water level in percentage.
- To control the water pump automatically using a relay.
- To provide manual pump control using push buttons.
- To generate a buzzer alert when the tank reaches a high water level.
- To detect ultrasonic sensor errors.
- To reduce water overflow and unnecessary water wastage.

---

## ⚙️ Features

- **Real-Time Water Level Monitoring:** Measures the distance between the sensor and the water surface.
- **LCD Display:** Displays the water level and pump status.
- **Automatic Mode:** Controls the pump according to predefined water-level thresholds.
- **Manual Mode:** Allows the user to control the pump using a push button.
- **Buzzer Alert:** Generates an alternating-frequency siren when the water level reaches 85%.
- **Sensor Error Detection:** Turns OFF the pump if the sensor reading is invalid.
- **Serial Monitor Output:** Displays distance, water level, pump status, operating mode, and buzzer status.

---

## 🛠️ Hardware Requirements

| Component | Quantity |
|---|---:|
| Arduino Mega 2560 / Compatible Arduino Board | 1 |
| HC-SR04 Ultrasonic Sensor | 1 |
| 16×2 I2C LCD Display | 1 |
| Relay Module | 1 |
| Water Pump | 1 |
| Buzzer | 1 |
| Push Buttons | 2 |
| Connecting Wires | As required |
| Breadboard | 1 |
| Power Supply | As required |

---

## 💻 Software Requirements

- Arduino IDE
- Embedded C/C++
- LiquidCrystal_I2C Library
- Wire Library
- Arduino Serial Monitor

---

## 🔌 Pin Configuration

| Component | Arduino Pin |
|---|---|
| Ultrasonic TRIG | Digital Pin 2 |
| Ultrasonic ECHO | Digital Pin 3 |
| SET Button | Digital Pin 10 |
| MODE Button | Digital Pin 11 |
| Buzzer | Digital Pin 12 |
| Relay Module | Digital Pin 13 |
| I2C LCD SDA | SDA |
| I2C LCD SCL | SCL |

**LCD Configuration:** I2C Address `0x27`, 16 Columns × 2 Rows.

For Arduino Mega 2560, the I2C pins are SDA = 20 and SCL = 21.

---

## 🔄 Working Principle

The system works in the following sequence:

1. The ultrasonic sensor sends ultrasonic waves toward the water surface.
2. The sensor receives the reflected waves.
3. The Arduino calculates the distance using the echo duration.
4. The water level is calculated in percentage.
5. The LCD displays the water level and pump status.
6. The Arduino controls the relay according to the selected operating mode.
7. The buzzer generates an alert when the water level reaches 85%.
8. The water level and system status are displayed on the Serial Monitor.

### Water Level Calculation

The water level is calculated using the following formula:

Water Level (%) = ((Tank Height − Distance) / Tank Height) × 100

In this project:

- Tank Height = 20 cm
- Distance = Distance measured by the ultrasonic sensor

The calculated value is limited between 0% and 100%.

---

## 🚰 Automatic Pump Control

The system uses two water-level thresholds:

| Water Level | Pump Action |
|---|---|
| 30% or Below | Pump turns ON |
| Between 30% and 85% | Pump maintains its previous state |
| 85% or Above | Pump turns OFF |
| Invalid Sensor Reading | Pump turns OFF |

This control method helps reduce unnecessary switching of the water pump.

---

## 🎛️ Manual Pump Control

The system also supports manual operation.

- Press the MODE button to switch between Automatic and Manual mode.
- In Manual mode, press the SET button to toggle the pump ON or OFF.
- In Automatic mode, the pump is controlled according to the water level.
- If the ultrasonic sensor reading becomes invalid, the pump is turned OFF.

---

## 🔔 Buzzer Alert System

The buzzer is activated when the water level reaches 85%.

The program generates an alternating-frequency siren:

- High frequency: 5000 Hz
- Low frequency: 1200 Hz
- Frequency changes approximately every 500 milliseconds.

This alert informs the user that the tank has reached its high-level threshold.

---

## 🖥️ Serial Monitor Output

The Arduino Serial Monitor displays:

- Distance in centimeters
- Water level in percentage
- Pump ON/OFF status
- Operating mode (AUTO/MANUAL)
- Buzzer ON/OFF status
- Sensor error messages

**Serial Baud Rate:** 9600

---

## 📷 Project Images

Project hardware images, circuit connections, and demonstration photographs can be added here.

<!-- Add your project images below -->

---

## 📂 Project Files

- [Arduino Source Code (PDF)](Arduino%20code.pdf)
- [Project Report (PDF)](Mini%20Project%20311.pdf)
- [README.md](README.md)

---

## 🚀 How to Run the Project

1. Install the Arduino IDE.
2. Install the LiquidCrystal_I2C library.
3. Connect the ultrasonic sensor, LCD, relay, buzzer, and push buttons according to the pin configuration.
4. Open the Arduino source code in the Arduino IDE.
5. Select the appropriate Arduino board and COM port.
6. Upload the code to the Arduino.
7. Open the Serial Monitor and set the baud rate to 9600.
8. Observe the water level, pump status, and buzzer alerts.

---

## 🔮 Future Scope

The project can be improved by adding:

- IoT-based remote water-level monitoring.
- Mobile application for monitoring and control.
- Cloud-based water-level data storage.
- SMS or mobile notifications.
- Water consumption monitoring.
- Multiple tank monitoring.
- Improved sensor accuracy using multiple readings.
- Pump dry-run protection.
- Additional safety mechanisms for pump operation.

---

## 🏁 Conclusion

The Water Tank Monitor project provides an effective solution for monitoring water levels and controlling a water pump.

The system uses an ultrasonic sensor to measure the water level and an Arduino to process the sensor data. The LCD displays the water level, while the relay controls the pump in automatic and manual modes.

The buzzer alert provides an additional warning when the tank reaches a high water level.

This project demonstrates the practical application of embedded systems, sensor technology, and automation in water management.

---

## 👨‍🎓 Project Information

**Project Name:** Water Tank Monitor  
**Course:** BSc Computer Science  
**Project Type:** College Mini Project  
**Technology:** Arduino, Embedded C/C++, Ultrasonic Sensor  
**GitHub Repository:** Water-Tank-Monitor
