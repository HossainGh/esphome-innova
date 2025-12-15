# 🌡️ Innova 2.0 HVAC - ESPHome Integration

[![ESPHome](https://img.shields.io/badge/ESPHome-000000?style=for-the-badge&logo=esphome&logoColor=white)](https://esphome.io/)
[![Home Assistant](https://img.shields.io/badge/home%20assistant-%2341BDF5.svg?style=for-the-badge&logo=home-assistant&logoColor=white)](https://www.home-assistant.io/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](https://opensource.org/licenses/MIT)

> **A rock-solid ESPHome firmware for Innova 2.0 HVAC units - Say goodbye to WiFi disconnections!**

## 🎯 Why This Project?

The original Innova 2.0 ESP firmware has a critical bug that causes frequent WiFi disconnections, making the unit unreachable on the network ([Issue #123](https://github.com/danielrivard/homeassistant-innova/issues/123), [Issue #41](https://github.com/danielrivard/innova-controls/issues/41)). Since the firmware is closed-source, fixing it was impossible.

**The solution?** Reverse-engineer the Modbus communication protocol and create a completely new, open-source firmware using ESPHome!

### ✨ What You Get

- ✅ **Stable WiFi connection** - No more random disconnections
- ✅ **Full Home Assistant integration** - Beautiful thermostat UI with all controls
- ✅ **Local control** - Built-in web interface, works without Home Assistant
- ✅ **Real-time updates** - 6-second polling interval for responsive control
- ✅ **OTA updates** - Update firmware over WiFi, no USB needed
- ✅ **Open source** - Customize and improve as needed

---

## 📋 Table of Contents

- [Hardware Requirements](#-hardware-requirements)
- [Quick Start Guide](#-quick-start-guide)
- [Detailed Installation](#-detailed-installation)
  - [Step 1: Install ESPHome CLI](#step-1-install-esphome-cli)
  - [Step 2: Prepare Your ESP Board](#step-2-prepare-your-esp-board)
  - [Step 3: Flash the Firmware](#step-3-flash-the-firmware)
  - [Step 4: Install the ESP Board](#step-4-install-the-esp-board)
- [Home Assistant Setup](#-home-assistant-setup)
- [Supported Features](#-supported-features)
- [Troubleshooting](#-troubleshooting)
- [Technical Details](#-technical-details)
- [Contributing](#-contributing)
- [Credits](#-credits)

---

## 🛠️ Hardware Requirements

### Required

- **Innova 2.0 HVAC Unit** with touch control panel
- **ESP-01S USB Programmer** - [Recommended Programmer](https://s.click.aliexpress.com/e/_msQI7tx) (~$2)

### Recommended

- **Spare ESP-01S board** - Keep your original ESP as backup (highly recommended!)

---

## 🚀 Quick Start Guide

> **⚠️ Important:** I strongly recommend buying a **new ESP-01S board** (~$2) and keeping your original ESP with the factory firmware as a backup!

### Overview

1. Install ESPHome CLI on your computer
2. Create a `secrets.yaml` file with your WiFi credentials
3. Flash the new firmware to ESP-01S
4. Replace the ESP board in your HVAC unit
5. Add to Home Assistant and enjoy!


---

## 📖 Detailed Installation

### Step 1: Install ESPHome CLI

ESPHome CLI allows you to compile and flash firmware to your ESP board.

**📚 Official Documentation:** [ESPHome Getting Started](https://esphome.io/guides/getting_started_command_line.html)

#### **Recommended option: Install via Python**

**Requirements:** Python 3.9 or newer

```bash
# Install ESPHome
pip install esphome

# Verify installation
esphome version
```

---

### Step 2: Prepare Your ESP Board

#### **2.1 Download This Repository**

```bash
git clone https://github.com/HossainGh/esphome-innova.git
cd esphome-innova
```

Or download as ZIP and extract.

#### **2.2 Create secrets.yaml**

Create a file named `secrets.yaml` in the same directory as the YAML file:

```yaml
# WiFi credentials
wifi_ssid: "YourWiFiName"
wifi_password: "YourWiFiPassword"

# Fallback AP credentials (when WiFi fails)
fallback_password: "innova12345"

# API encryption key (generate with: esphome dashboard .)
api_encryption_key: "GENERATE_YOUR_OWN_KEY"

# OTA password
ota_password: "innova-ota-password"
```

#### **2.3 Generate Encryption Key**

```bash
# Generate a random encryption key
esphome dashboard .
# The dashboard will generate a key for you
# Or use this command:
openssl rand -base64 32
```

Copy the generated key to your `secrets.yaml` file.

---

### Step 3: Flash the Firmware

#### **3.1 Connect ESP-01S to Programmer**

1. Insert ESP-01S into the USB programmer
2. This programmer automatically set ESP-01S to **"PROG"** mode. 
3. Plug the programmer into your computer's USB port

#### **3.2 Identify the Serial Port**

**Linux/Mac:**
```bash
ls /dev/tty.*
# Look for /dev/ttyUSB0 or /dev/cu.usbserial-XXXX
```

**Windows:**
- Open Device Manager
- Look under "Ports (COM & LPT)"
- Note the COM port (e.g., COM3, COM4)

#### **3.3 Compile and Flash**

```bash
# Compile the firmware
esphome compile hvac-innova.yaml

# Flash to ESP-01S (replace /dev/ttyUSB0 with your port)
esphome upload hvac-innova.yaml --device /dev/ttyUSB0
```

**Windows users:**
```bash
esphome upload hvac-innova.yaml --device [COM3]
```


**✅ Success!** Your ESP-01S is now running the new firmware.

---

### Step 4: Install the ESP Board

#### **4.1 Locate the ESP Board in Your HVAC Unit**

1. **Power off** the HVAC unit (unplug or turn off breaker)
2. Remove the front cover of the indoor unit
3. Locate the ESP-01S board (near the display PCB)
4. Take a photo of the board position for reference

#### **4.2 Remove the Original ESP Board**

1. Gently pull the ESP-01S board straight up to remove it from the socket
2. **Store it safely** - this is your backup!

#### **4.3 Install the New ESP Board**

1. Align the new ESP-01S with the socket pins
2. Ensure the orientation matches the original board
3. Firmly but gently press the board into the socket
4. Double-check that all pins are properly seated

#### **4.4 Power On and Test**

1. Power on the HVAC unit
2. Wait 30 seconds for the ESP to boot
3. Check your WiFi router - you should see a device named `hvac-innova`
4. Connect to the web interface: `http://hvac-innova.local` (or use the IP address)

**🎉 Congratulations!** Your HVAC is now running stable ESPHome firmware!

---

## 🏠 Home Assistant Setup

### Part 1: Install Template Climate Integration

#### **1.1 Install via HACS**

1. Open **HACS** in Home Assistant
2. Go to **Integrations**
3. Click **"+ EXPLORE & DOWNLOAD REPOSITORIES"**
4. Search for **"Climate Template"**
5. Click **Install**
6. **Restart** Home Assistant

**📦 Integration Repository:** [jcwillox/hass-template-climate](https://github.com/jcwillox/hass-template-climate)

#### **1.2 Configure Template Climate**

1. Open **Settings** → **Add-ons** → **File editor** → **OPEN WEB UI**
2. Navigate to `/homeassistant/configuration.yaml`
3. Add the following configuration at the end of the file:

```yaml
# ============================================================================
# INNOVA HVAC TEMPLATE CLIMATE
# ============================================================================
climate:
  - platform: climate_template
    name: "Innova HVAC"
    unique_id: innova_hvac_climate
    
    # Current temperature
    current_temperature_template: >
      {{ states('sensor.hvac_innova_hvac_ambient_temperature') | float(0) }}
    
    # Target temperature
    target_temperature_template: >
      {{ states('sensor.hvac_innova_hvac_setpoint_temperature') | float(24) }}
    
    # HVAC mode mapping
    hvac_mode_template: >
      {% set mode = states('sensor.hvac_innova_current_operating_mode') %}
      {% if mode == 'Cooling' %}
        cool
      {% elif mode == 'Heating' %}
        heat
      {% elif mode == 'Auto' %}
        auto
      {% elif mode == 'Fan Only' %}
        fan_only
      {% elif mode == 'Dehumidification' %}
        dry
      {% elif mode == 'Standby' %}
        off
      {% else %}
        off
      {% endif %}
    
    modes:
      - "off"
      - "heat"
      - "cool"
      - "auto"
      - "dry"
      - "fan_only"
    
    # Set HVAC mode
    set_hvac_mode:
      - service: select.select_option
        target:
          entity_id: select.hvac_innova_hvac_mode
        data:
          option: >
            {% if hvac_mode == 'cool' %}
              Cooling
            {% elif hvac_mode == 'heat' %}
              Heat
            {% elif hvac_mode == 'auto' %}
              Auto
            {% elif hvac_mode == 'dry' %}
              Dehumidification
            {% elif hvac_mode == 'fan_only' %}
              Fan Only
            {% endif %}
      - condition: template
        value_template: "{{ hvac_mode == 'off' }}"
      - service: switch.turn_off
        target:
          entity_id: switch.hvac_innova_hvac_power
    
    # Fan mode
    fan_mode_template: >
      {{ states('select.hvac_innova_fan_speed_mode') }}
    
    fan_modes:
      - "Auto"
      - "Speed 1"
      - "Speed 2"
      - "Speed 3"
      - "Turbo"
    
    set_fan_mode:
      - service: select.select_option
        target:
          entity_id: select.hvac_innova_fan_speed_mode
        data:
          option: "{{ fan_mode }}"
    
    # Swing mode
    swing_mode_template: >
      {% if is_state('switch.hvac_innova_hvac_rotation', 'on') %}
        on
      {% else %}
        off
      {% endif %}
    
    swing_modes:
      - "on"
      - "off"
    
    set_swing_mode:
      - service: >
          {% if swing_mode == 'on' %}
            switch.turn_on
          {% else %}
            switch.turn_off
          {% endif %}
        target:
          entity_id: switch.hvac_innova_hvac_rotation
    
    # Temperature control
    set_temperature:
      - service: number.set_value
        target:
          entity_id: number.hvac_innova_hvac_setpoint
        data:
          value: "{{ temperature }}"
    
    min_temp: 16
    max_temp: 30
    temp_step: 1
    precision: 1.0
    
    # Availability
    availability_template: >
      {{ not is_state('switch.hvac_innova_hvac_power', 'unavailable') }}
    
    # Current action
    hvac_action_template: >
      {% set mode = states('sensor.hvac_innova_current_operating_mode') %}
      {% set power = states('sensor.hvac_innova_current_power_mode') %}
      {% if power == 'Standby' %}
        idle
      {% elif mode == 'Cooling' %}
        cooling
      {% elif mode == 'Heating' %}
        heating
      {% elif mode == 'Dehumidification' %}
        drying
      {% elif mode == 'Fan Only' %}
        fan
      {% else %}
        idle
      {% endif %}
```

4. **Save** the file
5. Go to **Developer Tools** → **YAML** → **CHECK CONFIGURATION**
6. If valid, go to **Settings** → **System** → **RESTART**

### Part 2: Add Thermostat Card to HA Dashboard

1. Go to your dashboard
2. Click **Edit Dashboard** (three dots → Edit Dashboard)
3. Click **"+ ADD CARD"**
4. Click **By card**
5. Serach for **Thermostat** and select it
6. Click **Save**

**🎨 Done!** You now have a beautiful, fully-functional thermostat interface!

---

## 🎮 Supported Features

### Control Features

| Feature | Supported | Notes |
|---------|-----------|-------|
| Power On/Off | ✅ | Full control |
| Temperature Setpoint | ✅ | 16-30°C, 1° steps |
| Cooling Mode | ✅ | |
| Heating Mode | ✅ | |
| Auto Mode | ✅ | |
| Dehumidification | ✅ | |
| Fan Only Mode | ✅ | |
| Fan Speed Control | ✅ | Auto, 3 speeds, Turbo |
| Swing/Rotation | ✅ | On/Off |
| Night Mode | ✅ | |

### Monitoring Features

| Sensor | Entity ID | Update Rate |
|--------|-----------|-------------|
| Ambient Temperature | `sensor.hvac_innova_hvac_ambient_temperature` | 6s |
| Target Temperature | `sensor.hvac_innova_hvac_setpoint_temperature` | 6s |
| Operating Mode | `sensor.hvac_innova_current_operating_mode` | 6s |
| Power Status | `sensor.hvac_innova_current_power_mode` | 6s |
| Fan Speed | `sensor.hvac_innova_fan_speed_0_6` | 6s |
| Rotation Status | `sensor.hvac_innova_rotation_status` | 6s |

---

## 🔧 Troubleshooting

### ESP Board Issues

**Problem: ESP won't boot after installation**
- ✅ Check board orientation - match the original ESP position
- ✅ Ensure all pins are firmly seated in the socket
- ✅ Power cycle the HVAC unit

**Problem: WiFi connection fails**
- ✅ Check WiFi credentials in `secrets.yaml`
- ✅ Ensure 2.4GHz WiFi is enabled (ESP-01S doesn't support 5GHz)
- ✅ Move router closer or use a WiFi extender
- ✅ Connect to fallback AP: `Innova HVAC Fallback` (password from secrets.yaml)

**Problem: Can't flash ESP board**
- ✅ Check USB programmer switch is in "PROG" or "UART" mode
- ✅ Try a different USB cable or port
- ✅ Install USB driver for your programmer if needed
- ✅ Hold GPIO0 button while powering on (if using other programmer)

### Home Assistant Issues

**Problem: Climate entity not appearing**
- ✅ Check configuration.yaml syntax
- ✅ Verify all entity IDs match your ESPHome device
- ✅ Check Developer Tools → States for raw entities
- ✅ Restart Home Assistant after configuration changes

**Problem: Controls don't work**
- ✅ Verify ESP board has power (check web interface at `http://hvac-innova.local`)
- ✅ Check ESPHome logs: Settings → Add-ons → ESPHome → Logs
- ✅ Ensure Modbus communication is working (check logs for errors)

**Problem: Simple Thermostat card shows error**
- ✅ Ensure Simple Thermostat is installed via HACS
- ✅ Clear browser cache (Ctrl+F5)

### Getting Help

- 📝 **Open an Issue:** [GitHub Issues](https://github.com/HossainGh/esphome-innova/issues)
- 💬 **Discussions:** [GitHub Discussions](https://github.com/HossainGh/esphome-innova/discussions)
- 📚 **ESPHome Docs:** [esphome.io](https://esphome.io/)
- 🏠 **Home Assistant Community:** [community.home-assistant.io](https://community.home-assistant.io/)

---

## 🔬 Technical Details

### Communication Protocol

- **Protocol:** Modbus RTU over UART
- **Baud Rate:** 9600
- **Data Bits:** 8
- **Parity:** Even
- **Stop Bits:** 1
- **Slave Address:** 0x01
- **Function Codes:** FC03 (Read), FC06 (Write Single Register)

### Modbus Register Map

#### Read Registers (FC03)

| Register | Description | Values | Update Rate |
|----------|-------------|--------|-------------|
| 4098 | Setpoint Temperature | 16-30°C | 6s |
| 4099 | Rotation Status | 0=On, 7=Off | 6s |
| 4101 | Night Mode Status | 33=Normal, 34=Standby, 35=Night | 6s |
| 4129 | Ambient Temperature | Temperature in °C | 6s |
| 4139 | Operating Mode | 0=Auto, 1=Cool, 2=Dehumid, 4=Heat, 6=Fan, 8=Standby | 6s |
| 4140 | Fan Speed | 0=Auto/Standby, 2/3/6=Speeds | 6s |

#### Write Registers (FC06)

| Register | Description | Values |
|----------|-------------|--------|
| 1 | Mode / Power | 0=Heat, 1=Cool, 2=PowerOff, 3=Dehumid, 4=Fan, 5=Auto |
| 5 | Setpoint Temperature | 16-30 (decimal) |
| 6 | Fan Speed | 0=Auto, 1-3=Speeds, 4=Turbo |
| 7 | Rotation | 0=On, 7=Off |
| 8 | Night Mode | 0=Off, 1=On |

### Hardware Specifications

**Main Controller:** Renesas R5F513T3ADFJ  
**WiFi Module:** ESP-01S (ESP8266)  
**Connection:** 8-pin header (TX, RX, VCC, GND, GPIO0, GPIO2, RST, CH_PD)

### Reverse Engineering Process

This project was made possible through:
1. **Logic Analyzer Capture** - Recorded UART communication between ESP and main controller
2. **Protocol Analysis** - Identified Modbus RTU protocol and register mappings
3. **Testing & Validation** - Verified all functions work correctly with the new firmware

---

## 🤝 Contributing

Contributions are welcome! Whether it's bug reports, feature requests, or code improvements, we'd love your help.

### How to Contribute

1. **Fork** this repository
2. **Create** a feature branch (`git checkout -b feature/amazing-feature`)
3. **Commit** your changes (`git commit -m 'Add amazing feature'`)
4. **Push** to the branch (`git push origin feature/amazing-feature`)
5. **Open** a Pull Request

---

## 📜 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---

## 🙏 Credits

### Original Integration

- **danielrivard** - Original Innova Home Assistant integration  
  [homeassistant-innova](https://github.com/danielrivard/homeassistant-innova)

### Tools & Libraries

- **ESPHome** - Amazing ESP firmware framework  
  [esphome.io](https://esphome.io/)
- **Home Assistant** - Open source home automation  
  [home-assistant.io](https://www.home-assistant.io/)
- **Template Climate** - Climate template integration  
  [jcwillox/hass-template-climate](https://github.com/jcwillox/hass-template-climate)

### Community

Thanks to everyone who reported issues, tested solutions, and contributed to making this project possible!

---

## ⭐ Support This Project

If this project helped you, please consider:

- ⭐ **Starring** this repository
- 🐛 **Reporting** bugs and issues
- 💡 **Suggesting** new features
- 🔀 **Contributing** code improvements
- 📢 **Sharing** with others who have Innova 2.0 units

---

## 📞 Contact

- **GitHub Issues:** [Open an issue](https://github.com/HossainGh/esphome-innova/issues)
- **Discussions:** [Start a discussion](https://github.com/HossainGh/esphome-innova/discussions)

---

<div align="center">

**Made with ❤️ for the Innova 2.0 community**

*No more WiFi disconnections! 🎉*

</div>