# ESP32 Smart Home Automation System

An ESP32-based embedded project that combines RFID door access, temperature/humidity display, rain-triggered servo controls, flame detection with a buzzer alarm, and sensor-driven LED control. The source organizes each function into a separate C++ class and calls their setup and processing methods from the Arduino `setup()` and `loop()` functions.

## Features

- **RFID door access:** Reads RFID tags with an MFRC522 reader and toggles a servo between 0° and 90° for two hard-coded authorized UIDs.
- **Temperature and humidity display:** Reads a DHT11 sensor and displays the values on a 16×2 I2C LCD.
- **Rain response:** Detects a digital rain-sensor state change and moves two servos to close/open positions.
- **Flame alarm:** Detects an active-low flame sensor and sweeps the buzzer tone between 500 Hz and 2000 Hz while a flame is detected.
- **Sensor-driven LED:** Changes an LED state when the digital input connected to the light-sensor pin changes state.
- **Class-based organization:** Separates RFID door control, LCD/sensor display, rain response, flame alarm, and LED control into classes.

## Hardware and Libraries

The code includes these Arduino libraries:

- `SPI.h`
- `MFRC522.h`
- `ESP32Servo.h`
- `DHT.h`
- `LiquidCrystal_I2C.h`
- `WiFi.h` (included in the source but not used by the shown functionality)

The source is configured for an ESP32 and defines pins for an MFRC522 RFID reader, servos, DHT11 sensor, rain sensor, flame sensor, buzzer, LED, I2C LCD, and a digital light-sensor input. Check the exact ESP32 board variant and pin compatibility before wiring; pin assignments may need to change for your board.

## Pin Configuration in the Source

| Function | Pin(s) |
|---|---|
| RFID SS / SCK / MOSI / MISO / RST | 10 / 11 / 12 / 13 / 9 |
| Door servo | 17 |
| I2C SDA / SCL | 8 / 3 |
| DHT11 data | 18 |
| Rain sensor | 4 |
| Window servo | 15 |
| Second rain-controlled servo | 16 |
| Flame sensor | 2 |
| Buzzer | 37 |
| LED | 6 |
| Digital light-sensor input | 35 |

These are source-code assignments, not a guarantee that the pins are supported on every ESP32 model.

## Setup

1. Install a compatible ESP32 Arduino board package in the Arduino IDE.
2. Install the required libraries listed above.
3. Wire the modules according to the pin definitions, checking voltage requirements and the board's pin capabilities.
4. Upload the sketch to the ESP32 and open Serial Monitor at **115200 baud**.
5. Test each sensor and actuator independently before running the whole system.

## Important Implementation Details

- The RFID access list is hard-coded in the source. For a real installation, avoid treating a card UID alone as strong security, and do not publish any real access identifiers if they are sensitive.
- The rain sensor is interpreted as active-low (`LOW` means rain detected).
- The flame sensor is interpreted as active-low (`LOW` means flame detected).
- The LCD uses I2C address `0x27`.
- The light-sensor input is handled as a digital state change; the source comments call this motion detection, so verify which sensor is actually connected.

## Limitations and Safety Notes

- The included `WiFi.h` is not used in the code shown; network connectivity or remote control is not implemented.
- `DHTLCD.process()` contains a two-second delay, and the flame-alarm sweep contains blocking delays. These can delay processing of other functions in the main loop.
- The alarm is a hobby-project demonstration and is not a certified fire detection system. Do not use it as a replacement for approved smoke/fire alarms.
- Servo positions and sensor active levels should be calibrated to the actual hardware.
- The code shown does not include a schematic, enclosure design, or detailed power plan. Use suitable power supplies and common ground where required.

## Technologies

C++, Arduino framework, ESP32, MFRC522 RFID, DHT11, I2C LCD, servos, rain sensor, flame sensor, buzzer, digital sensor input.
