# Raspberry Pi Automated Lighting with LDR and ADS1115

This project uses a Raspberry Pi, an ADS1115 Analog-to-Digital Converter (ADC), and a Light Dependent Resistor (LDR) to monitor ambient light levels. When the light drops below a customizable threshold, the script automatically triggers an LED.

## Hardware Requirements

* **Raspberry Pi** (e.g., Pi 3, Pi 4, or Pi 5) with Raspberry Pi OS.
* **ADS1115** 16-Bit ADC Module.
* **LDR (Photoresistor)**.
* **LED** (Any standard 5mm or 3mm LED).
* **Resistors:**
  * 1x 10kΩ resistor (for the LDR voltage divider).
  * 1x 220Ω or 330Ω resistor (current-limiting for the LED).
* **Breadboard and Jumper Wires**.

## Wiring Guide

### 1. ADS1115 ADC to Raspberry Pi (I2C)
* **VDD** -> Pi 3.3V (Physical Pin 1 or 17)
* **GND** -> Pi GND (Physical Pin 6, 9, 14, etc.)
* **SCL** -> Pi SCL (Physical Pin 5 / GPIO 3)
* **SDA** -> Pi SDA (Physical Pin 3 / GPIO 2)

### 2. LDR Sensor Circuit (Voltage Divider)
* Connect one leg of the **LDR** to the Pi's **3.3V**.
* Connect the other leg of the **LDR** to **A0** on the ADS1115.
* Connect the **10kΩ resistor** from **A0** on the ADS1115 to **GND**.

### 3. LED Output
* Connect the **Anode** (longer leg) of the **LED** to **GPIO 16** (Physical Pin 36).
* Connect the **Cathode** (shorter leg) of the **LED** to one end of the **220Ω resistor**.
* Connect the other end of the **220Ω resistor** to **GND**.

## Software Setup

1. **Enable I2C:** Ensure I2C is enabled on your Raspberry Pi. You can do this by running `sudo raspi-config`, navigating to **Interface Options**, and enabling **I2C**.
2. **Update Packages:**
   ```bash
   sudo apt update
   sudo apt upgrade
   ```
3. **Install Dependencies:** Install the required Adafruit CircuitPython libraries.
   ```bash
   pip3 install adafruit-blinka
   pip3 install adafruit-circuitpython-ads1x15
   ```

## Developing in Visual Studio Code (VS Code)

If you are using VS Code to write and execute this project, following these steps will provide the best experience:

1. **Remote Connection (Recommended):** If you are coding from a separate PC/Mac, install the **Remote - SSH** extension in VS Code. This allows you to connect directly to your Raspberry Pi, edit files, and use the Pi's terminal natively from your main computer.
2. **Extensions:** Install the official **Python** extension by Microsoft. If using Remote - SSH, ensure it is installed on the SSH target (the Pi).
3. **Select Interpreter:** Press `Ctrl+Shift+P` (or `Cmd+Shift+P` on Mac), search for `Python: Select Interpreter`, and select the environment where you installed the `adafruit-blinka` libraries (usually the default `/usr/bin/python3`).
4. **Running the Script:** 
   * **Option 1 (Terminal):** Open the integrated terminal (`Ctrl + \``) and type `python3 ldr_light.py`.
   * **Option 2 (UI):** Click the "Play" button (Run Python File) in the top-right corner of the editor window.

## Usage

1. Save the provided Python script as `ldr_light.py`.
2. Run the script. The terminal will display the raw ADC values and voltage.
3. **Calibration:** Cover the LDR with your hand to simulate darkness. Note the raw value printed in the terminal, and update the `DARK_THRESHOLD` variable in your script so it accurately triggers the LED in your specific environment.

## Troubleshooting

* **LED never turns on:** Double-check your LED polarity (long leg to GPIO, short leg to ground) and verify it is connected to Physical Pin 36 (GPIO 16).
* **I2C Errors:** Run `i2cdetect -y 1` in the terminal to verify the Pi recognizes the ADS1115 (usually at address `0x48`).
* **VS Code "ModuleNotFoundError":** If VS Code underlines `import board` in yellow, ensure you have correctly selected your Python interpreter as outlined in the VS Code Setup section.