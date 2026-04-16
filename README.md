# IoT-Air_Pollution_Monitoring_System
The IoT Air Pollution Monitoring System is a prototype developed as part of the CSF3313 IoT Computing course at Universiti Malaysia Terengganu (UMT). It addresses the gap in affordable, real-time air quality monitoring by combining low-cost hardware with cloud-based visualization and instant notifications.
Traditional air quality monitoring systems are expensive, stationary, and inaccessible to small communities. This project bridges that gap with a portable, cost-effective solution that anyone- from individuals to policymakers- can use to understand and react to their air quality environment.

# Technology 
# Hardware
- ESP32 Microcontroller - Central processing unit with built-in Wi-Fi
- MQ-7 Gas Sensor - Carbon Monoxide (CO) detection via analog ADC
- CCS811 Gas Sensor - CO₂ and TVOC detection via I2C
- DHT11 Sensor - Temperature and Humidity readings
- Breadboard + WiringCircuit assembly

# Software
- MicroPython - Firmware language for ESP32
- UpyCraft IDE - Development and flashing environment
- FavorIoT Cloud - Real-time data storage, streaming and dashboard visualization
- MQTT Protocol - Lightweight data transmission to cloud
- Telegram Bot API - Real-time push notifications for pollution alerts

# ✨ Features
- 📡 Real-Time Monitoring — Continuously reads CO, CO₂, TVOC, temperature, and humidity every 60 seconds
- ☁️ Cloud Integration — Streams all sensor data to FavorIoT dashboard via MQTT in JSON format
- 📊 Data Visualization — Interactive FavorIoT dashboard with timestamped data streams and trend analysis
- 🔔 Instant Telegram Alerts — Pushes notifications directly to your phone when pollution is detected
- 🏷️ Pollution Classification — Automatically labels readings as No Pollution, Moderate Pollution, or High Pollution
- 🔁 Wi-Fi Auto-Reconnect — Retries connection up to 10 times to ensure reliability in unstable networks
- 💰 Affordable — Total cost of only MYR 105.82, making it accessible to communities, schools, and individuals
= 🔌 Portable & Modular — Breadboard-based design that can be deployed in various environments

# ⌨️ Keyboard Shortcuts
This project runs on embedded hardware (ESP32). The following shortcuts apply within the UpyCraft IDE development environment.
- F5 = Run current script on device
- Ctrl + S = Save current file
- Ctrl + Z = Undo last action
- Ctrl + / = Toggle line comment
- Ctrl + F = Find in file
- Ctrl + Shift + D = Download script to ESP32
- Ctrl + R = Restart ESP32 device
- Ctrl + C = Interrupt running script (in serial terminal)

# 🔄 The Process
The system follows a structured pipeline from sensor reading to user notification:
┌─────────────────────────────────────────────────────────┐
│                        START                            │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│         Initialize Hardware (ESP32 + Sensors)           │
│         MQ-7 on GPIO34, CCS811 on I2C, DHT11 on GPIO4  │
└────────────────────────┬────────────────────────────────┘
                         │
                         ▼
┌─────────────────────────────────────────────────────────┐
│       Connect to Wi-Fi (retry up to 10 attempts)        │
└────────────────────────┬────────────────────────────────┘
                         │
              ┌──────────┴──────────┐
              │ Connected?          │
              │                     │
             YES                    NO
              │                     │
              ▼                     ▼
┌────────────────────┐   ┌─────────────────────────┐
│  Read Sensor Data  │   │  Log Failure & Retry or  │
│  MQ-7, CCS811,     │   │  Exit after max retries  │
│  DHT11             │   └─────────────────────────┘
└────────┬───────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────┐
│           Process & Validate Data                       │
│     classify_pollution(co_ppm, co2, tvoc)               │
│     → "No Pollution" / "Moderate" / "High"              │
└────────────────────────┬────────────────────────────────┘
                         │
              ┌──────────┴──────────┐
              │ Data Valid?         │
              │                     │
             YES                    NO
              │                     │
              ▼                     ▼
┌────────────────────┐   ┌──────────────────────────┐
│  Publish to MQTT   │   │  Log Error, Skip Cycle   │
│  (FavorIoT Cloud)  │   └──────────────────────────┘
└────────┬───────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────┐
│          Visualize on FavorIoT Dashboard                │
└────────────────────────┬────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────┐
│   If Moderate/High Pollution → Send Telegram Alert      │
└────────────────────────┬────────────────────────────────┘
         │
         ▼
┌─────────────────────────────────────────────────────────┐
│             Wait 60 seconds → Repeat                    │
└─────────────────────────────────────────────────────────┘

# 📚 What I Learned

- 🔌 How to wire and read data from multiple sensors (analog, I2C, digital) on a single ESP32
- 🐍 Writing MicroPython firmware with proper error handling and Wi-Fi retry logic
- 📡 How MQTT works for lightweight IoT data transmission to the cloud
- ☁️ Sending and visualizing sensor data on the FavorIoT platform in real time
- 🤖 Integrating a Telegram Bot API to send automated pollution alerts
- 🧪 The importance of sensor calibration and data validation before acting on readings
- 🤝 Collaborating as a team by splitting responsibilities and syncing through weekly meetings

# 🚀 How It Could Be Improved
While the prototype successfully demonstrates the core concept, several enhancements could make it production-ready:

 - Add more sensors (PM2.5, NO₂) to detect a wider range of pollutants
 - Replace Wi-Fi with LoRaWAN or NB-IoT for use in rural areas without internet access
 - Add a GPS module for location-tagged, mobile monitoring
 - Build a mobile app for a better user experience beyond the dashboard
 - Design a custom PCB and weatherproof casing for outdoor deployment
 - Test high pollution scenarios to validate upper threshold accuracy
 - Support multiple devices to build a network of monitoring nodes across a wider area

# ▶️ How to Run the Project
Prerequisites

- ESP32 development board
- MQ-7, CCS811, and DHT11 sensors
- USB cable (for flashing)
- UpyCraft IDE installed
- FavorIoT account — favoriot.com
- Telegram account + a Telegram Bot (created via @BotFather)

# Step 1 — Hardware Setup
- Wire the components to your ESP32 as follows:
  DHT11:
  VCC → 3.3V
  GND → GND
  DATA → GPIO 4
- MQ-7:
  VCC → 5V
  GND → GND
  AOUT → GPIO 34
- CCS811:
  VCC → 3.3V
  GND → GND
  SDA → GPIO 21
  SCL → GPIO 22
  WAKE → GND (always active)

# Step 2 — Configure Your Credentials
- Open main.py and update the following variables:
- Wi-Fi
  wlan.connect("YOUR_WIFI_SSID", "YOUR_WIFI_PASSWORD")

- FavorIoT MQTT
  SERVER = "mqtt.favoriot.com"
  client = MQTTClient("umqtt_client", SERVER,
    user="YOUR_FAVORIOT_API_KEY",
    password="YOUR_FAVORIOT_API_KEY")

- MQTT Topic
  topic = "YOUR_FAVORIOT_API_KEY/v2/streams"

- Telegram
   TELEGRAM_BOT_TOKEN = "YOUR_BOT_TOKEN"
   TELEGRAM_CHAT_ID = "YOUR_CHAT_ID"

# Step 3 — Flash MicroPython to ESP32
- Download the latest MicroPython firmware for ESP32
- Flash it using esptool:
  esptool.py --chip esp32 erase_flash
  esptool.py --chip esp32 --baud 460800 write_flash -z 0x1000 esp32-firmware.bin

# Step 4 — Upload the Code
- Open UpyCraft IDE
- Connect your ESP32 via USB
- Upload the main.py file to the device
- Press Run (F5) or reset the ESP32

# Step 5 — Monitor Output
- Watch the serial terminal in UpyCraft for live readings:
 Network connected: ('192.168.x.x', '255.255.255.0', ...)
 Starting sensor monitoring...
 Temp: 25.0C, Humidity: 50.0%, CO: 1.56ppm, CO2: 400ppm, TVOC: 50ppb
 Pollution Level: No Pollution

- Open your FavorIoT Dashboard to view real-time data streams and trends!

# 🎥 Demo
- ▶️ Watch the YouTube Demo : https://youtu.be/yf5weHHt9VU?si=puxOpZGJsUVWxTJ1

