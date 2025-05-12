## **Heater Control System**
LINK: https://wokwi.com/projects/430769068522654721


An ESP32‐based heater controller that:

1. Reads temperature from a DS18B20 sensor
2. Runs a 5-state finite state machine (FSM):

   * Idle
   * Heating
   * Stabilizing
   * Target Reached
   * Overheat
3. Controls a heater indicator (LED) and an audible alarm (buzzer)
4. Broadcasts the current FSM state over Bluetooth Low Energy (BLE) as manufacturer data
5. Logs temperature and state over Serial every second

---

## Features

* **Finite State Machine** logic for smooth temperature control
* **DS18B20** digital temperature sensing (±0.5 °C accuracy)
* **BLE Advertising** of `[state, ~state]` packet—no pairing required
* **Serial Logging** at 115 200 baud for real-time monitoring
* **Plug-and-play**: Uses Arduino-style APIs and libraries

---

## Hardware Connections

| Component      | ESP32 Pin | Notes                                 |
| -------------- | --------- | ------------------------------------- |
| DS18B20 DATA   | GPIO 25   | With 4.7 kΩ pull-up resistor to 3.3 V |
| DS18B20 VCC    | 3.3 V     |                                       |
| DS18B20 GND    | GND       |                                       |
| Heater LED (+) | GPIO 16   | Through 220 Ω resistor to GND         |
| Buzzer (+)     | GPIO 17   | Buzzer (–) to GND                     |

---

## Dependencies

* **Arduino Core for ESP32**
* **OneWire** library
* **DallasTemperature** library
* **BLEDevice**, **BLEUtils**, **BLEServer** (built into ESP32 Arduino core)

Install libraries via **Arduino Library Manager** if needed.

---

## Sketch Structure

1. **Definitions & Includes**

   * Pin assignments, thresholds, FSM enum.
2. **Setup()**

   * Initialize Serial, DS18B20, GPIOs, BLE advertising.
3. **Loop()** (runs every 1 s)

   * Read temperature
   * Update FSM & actuators
   * Log to Serial
   * Update BLE advertisement with `[state, ~state]`
4. **FSM Logic** (`updateStateAndActuate`)

   * Transitions based on thresholds
   * Controls LED & buzzer outputs
5. **Helper** (`stateName`)

   * Returns human-readable state string

---

## Usage

1. **Wire your hardware** as shown above.
2. **Flash** the sketch to your ESP32 at 115 200 baud.
3. **Open** Serial Monitor (115 200 baud) to observe temperature & state logs.
4. **Use** any BLE scanner (e.g., **nRF Connect**) to scan for “HeaterFSM” and view the manufacturer data—first byte is the state (0=Idle, 1=Heating, 2=Stabilizing, 3=Target Reached, 4=Overheat).


## Future Enhancements

* Add **GATT services** for remote configuration (target temperature, profiles).
* Implement a **PID controller** for smoother temperature regulation.
* Integrate **overheat hardware cutoff** for added safety.
* Expand to **Wi-Fi + cloud** for data logging and remote alerts.




