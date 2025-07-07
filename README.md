# arduino-fpga-smart-irrigation
An automated system built using arduino, FPGA, and pyhton.

This project implements an intelligent home automation system for plant care, integrating environmental sensing, hardware control, and a machine learning model for optimal watering recommendations, displayed via a custom VGA output on an FPGA.

## Project Overview

**arduino-fpga-smart-irrigation** is a real-time embedded system designed to automate plant care by monitoring environmental conditions (soil moisture, humidity), controlling actuators (water pump, fan), and providing visual feedback. It leverages a hybrid architecture combining a Raspberry Pi Pico microcontroller for sensing and control with a DE10-Lite FPGA for real-time VGA display of sensor data.

## Demo

Check out a demonstration of the system in action:
https://www.youtube.com/watch?v=Yn1OlFF0FPE

## Goals and Rationale

The primary goal is to design and implement an intelligent system that provides personalized care for plants, enhancing health and reducing risks of over/under-watering. The rationale is rooted in addressing challenges in agricultural applications, conservation, and home gardening. Engineering goals include contributing to biodiversity preservation and promoting environmental sustainability.

## System Architecture

The system employs a **dual-controller architecture**:

1.  **Raspberry Pi Pico:** Acts as the sensing and control hub. It interfaces with sensors, processes readings, implements threshold-based control logic for actuators, and encodes sensor data into a binary format for the FPGA.
2.  **DE10-Lite FPGA:** Handles the visual feedback. It receives binary sensor data from the Pico via GPIO, decodes it, converts it to BCD format, and renders real-time humidity and moisture values as 7-segment digits on a standard VGA monitor.

Communication between the Pico and DE10-Lite is achieved using **parallel binary GPIO**.

## Hardware Components

*   **Raspberry Pi Pico:** Microcontroller for sensor reading and actuator control.
*   **Intel DE10-Lite:** FPGA for VGA display output.
*   **DHT11 Sensor:** Measures ambient humidity and temperature.
*   **Analog Soil Moisture Sensor:** Measures water content in soil.
*   **5v Relay Module:** Controls the water pump.
*   **MOSFET Switch:** Optionally controls a cooling fan.
*   **Small Water Pump:** Delivers water to the plant.
*   **Breadboard, Jumpers, Power Supplies:** For system assembly and power distribution.

## Software and AI Model

*   **Raspberry Pi Pico Firmware:** Written in Arduino-style C++. Manages sensor readings, implements control logic, and encodes data for FPGA transmission.
*   **DE10-Lite FPGA Logic:** Developed in Verilog. Decodes incoming binary GPIO data, performs BCD conversion, and generates VGA signals to display sensor values. Includes custom logic for rendering 7-segment digits.
*   **AI Model (Python):** A time series forecasting model built using TensorFlow/Keras.
    *   **Architecture:** Long Short-Term Memory (LSTM) network.
    *   **Input Features:** Moisture, Humidity, Watering levels from historical data.
    *   **Target:** Predicted Plant Health score.
    *   **Training Data:** Concatenated data from five distinct environmental scenarios.
    *   **Preprocessing:** MinMaxScaler for normalization, sliding window approach for sequence creation.
    *   **Optimization Strategy:** Randomized search to find environmental conditions that maximize the predicted plant health for the next day.

The AI model is separate from the real-time embedded system.

## Experimentation and Results

The system was tested under various conditions.

Key findings:
*   The soil moisture and humidity thresholds effectively triggered the water pump and fan.
*   Real-time environmental feedback was successfully displayed on the VGA monitor via the DE10-Lite with minimal perceived latency.
*   The parallel binary GPIO communication scheme proved functional for data transfer.
*   The system demonstrated reliable closed-loop sensing and actuation.

## Challenges Encountered

*   **Unstable GPIO Data Transfer:** Inconsistent connections via breadboard jumpers caused brief flickering on the VGA display.
*   **Timing Mismatch:** The difference between the Pico's sensor update rate and the VGA refresh rate caused temporary visual glitches.
*   **Voltage and Power Issues:** Powering high-current actuators from the same rail as logic components caused voltage dips, requiring mitigation.
*   **Moisture Sensor Inconsistency:** Sensor readings were sensitive to soil type and environmental noise, necessitating a buffered threshold.

Most challenges were addressed through hardware adjustments and logic refinements.

## Getting Started

To replicate this project, you will need the hardware listed above and the corresponding software environments:
*   Arduino IDE (for Pico code)
*   Intel Quartus Prime (for DE10-Lite Verilog code)
*   Python with necessary libraries (TensorFlow, Keras, scikit-learn, pandas, numpy, matplotlib) for the AI model.

The final code for the Pico (Arduino C++), DE10-Lite (Verilog), and the AI Model (Python) are available in this repository, in dedicated folders (e.g., `Verilog`, `Arduino`, `ML`). Refer to the code comments and the project documentation for detailed setup and usage instructions.
