# Semester Project Proposal: Smart Fermentation Chamber Controller

## Student Information
- **Name:** EMMANUEL FRIMPONG
- **Course:** TELE6530 - CONNECTED DEVICES
- **Semester:** FALL 2025
- **Project Type:** Semester Project (Lab Module 12)

---

## 1. Executive Summary

This project implements an automated fermentation chamber monitoring and control system designed for home brewing and fermentation applications. The system continuously monitors temperature, humidity, and atmospheric pressure within a fermentation chamber, automatically adjusting environmental conditions to maintain optimal parameters for various fermentation types (ale, lager, wine, kombucha, etc.). The solution integrates a Constrained Device Application (CDA), Gateway Device Application (GDA), and cloud services to provide real-time monitoring, automated control, and remote access capabilities.

---

## 2. Problem Statement

Home brewing and fermentation require precise environmental control to ensure quality results and prevent batch failures. Key challenges include:

**Temperature Control:** Fermentation is highly temperature-sensitive. Yeast strains have specific optimal temperature ranges (e.g., ale yeast: 68-72°F, lager yeast: 45-55°F). Temperature fluctuations of even a few degrees can:
- Kill yeast cultures
- Produce off-flavors (fusel alcohols, esters)
- Stall fermentation prematurely
- Reduce final product quality

**Humidity Management:** Proper humidity levels (50-70%) are critical during conditioning and aging phases to:
- Prevent bottle corks from drying and allowing oxidation
- Maintain proper conditions for barrel aging
- Ensure consistent evaporation rates

**Continuous Monitoring Requirements:** Fermentation cycles last 2-4 weeks, making manual 24/7 monitoring impractical. Brewers need:
- Automated response to condition changes
- Historical data to analyze fermentation progress
- Remote monitoring capabilities
- Alerts for dangerous conditions

**Cost of Failure:** A failed fermentation batch represents significant losses:
- 4-8 hours of brew day labor
- $50-150 in ingredients per batch
- 2-4 weeks of time investment
- Loss of anticipated product

**Current Solutions are Inadequate:**
- Manual monitoring requires constant presence
- Simple thermostats lack humidity control and data logging
- Commercial fermentation chambers cost $500-2000+
- Existing solutions lack remote monitoring and historical analysis

---

## 3. Proposed Solution

### 3.1 Solution Overview

An end-to-end IoT system that automates fermentation chamber environmental control through:

1. **Continuous Environmental Monitoring:** Real-time collection of temperature, humidity, and pressure data
2. **Automated Climate Control:** Intelligent actuation of cooling, heating, and humidification based on fermentation profiles
3. **Visual Status Indication:** LED display showing current conditions and system status
4. **Remote Monitoring & Control:** Cloud-based dashboard for real-time monitoring and manual overrides
5. **Historical Data Analysis:** Time-series data storage enabling fermentation progress tracking and trend analysis
6. **Multi-Profile Support:** Configurable temperature/humidity profiles for different fermentation types

### 3.2 System Architecture

The solution consists of three integrated tiers:

**Tier 1: Constrained Device Application (CDA) - Python**
- Deployed on edge device (Raspberry Pi or similar)
- Collects sensor data (temperature, humidity, pressure)
- Controls actuators (HVAC, humidifier, LED display)
- Implements local threshold-based automation
- Communicates with GDA via secure MQTT or CoAP

**Tier 2: Gateway Device Application (GDA) - Java**
- Receives and aggregates data from CDA
- Persists time-series data to Redis database
- Implements advanced fermentation logic and profile management
- Forwards data to cloud services via MQTT
- Relays remote commands from cloud to CDA

**Tier 3: Cloud Services**
- MQTT broker for data ingestion and command distribution
- Real-time monitoring dashboard
- Historical data visualization
- Remote control interface
- Alert notifications

### 3.3 Key Features

**Automated Environmental Control:**
- Maintain temperature within ±2°F of target setpoint
- Maintain humidity within ±5% of target range
- Automatic profile selection (ale, lager, conditioning, cold crash)
- Multi-stage fermentation support (primary, secondary, conditioning)

**Intelligent Monitoring:**
- Fermentation activity detection via temperature and pressure trends
- Automatic stage progression detection
- Anomaly detection (temperature spikes indicating infection)
- System performance monitoring (CPU, memory utilization)

**Safety & Alerts:**
- Immediate alerts for out-of-range conditions
- LED visual status indicators (green=optimal, yellow=active, red=alert)
- Remote notifications via cloud dashboard
- Failsafe mechanisms to prevent equipment damage

**Data Analysis:**
- Historical temperature curves over fermentation cycle
- Pressure trend analysis (CO2 production rates)
- Fermentation completion predictions
- Performance optimization insights

---

## 4. Objectives

### 4.1 Primary Objectives

1. **Implement Comprehensive Sensor Integration**
   - Deploy temperature, humidity, and pressure sensors
   - Achieve sampling rate of 1 reading per 2 minutes minimum (30+ samples/hour)
   - Ensure sensor data accuracy within ±1°F (temperature), ±3% (humidity)

2. **Develop Multi-Actuator Control System**
   - Implement HVAC control for heating/cooling
   - Implement humidifier control for moisture management
   - Implement LED display for visual status indication
   - Support at least 2 different actuator event types

3. **Create Local Intelligence Layer**
   - Implement threshold-based automation in CDA
   - Respond to sensor threshold crossings within 30 seconds
   - Maintain target temperature: 68-72°F (ale) or 50-55°F (lager)
   - Maintain target humidity: 50-70%

4. **Establish Secure Communication**
   - Encrypt CDA ↔ GDA communication using TLS (MQTT) or DTLS (CoAP)
   - Implement reliable message delivery
   - Handle connection failures gracefully

5. **Enable Remote Monitoring & Control**
   - Forward sensor data to cloud in real-time
   - Accept and process actuation commands from GDA/cloud
   - Support manual temperature profile changes
   - Provide emergency override capabilities

6. **Ensure System Reliability**
   - Run continuously for 1+ hour without interruption
   - Maintain system performance (CPU < 80%, Memory < 75%)
   - Log all events for troubleshooting and analysis

### 4.2 Secondary Objectives

1. **Fermentation Stage Detection**
   - Automatically identify lag phase, active fermentation, and conditioning stages
   - Adjust monitoring frequency based on fermentation activity

2. **Historical Data Persistence**
   - Store all sensor readings with timestamps in Redis
   - Enable date range queries for fermentation cycle analysis
   - Retain data for post-fermentation review

3. **Comprehensive Testing**
   - Develop unit tests for all new functionality
   - Create integration tests for end-to-end workflows
   - Validate system behavior under simulated fermentation conditions

---

## 5. Expected Outcomes

### 5.1 Functional Outcomes

**Environmental Control Performance:**
- Temperature maintained within ±2°F of target setpoint 95%+ of the time
- Humidity maintained within ±5% of target range 90%+ of the time
- Threshold crossings detected and addressed within 30 seconds
- Actuator commands executed within 10 seconds of receipt

**System Reliability:**
- 99%+ uptime during extended operation (1+ hour test)
- Zero crashes or unhandled exceptions
- All sensor samples successfully transmitted to GDA
- All actuation commands successfully processed

**Data Collection:**
- 30+ temperature samples per hour
- 30+ humidity samples per hour
- 30+ pressure samples per hour
- 30+ system performance samples per hour
- Complete time-series data available for analysis

**Actuation Events:**
- Minimum 2 distinct actuator events triggered during 1-hour test
- Local threshold-based actuations functioning correctly
- Remote actuation commands from GDA/cloud processing successfully

### 5.2 Technical Outcomes

**CDA Implementation:**
- Enhanced sensor data collection and processing
- Advanced actuator control logic supporting multiple event types
- Fermentation-specific threshold implementations
- Improved LED display showing real-time metrics
- Robust error handling and recovery mechanisms

**GDA Implementation:**
- Fermentation profile management (ale, lager, conditioning)
- Time-series data persistence in Redis
- Trend analysis and fermentation stage detection
- Enhanced actuation command generation and relay
- Comprehensive event logging

**Cloud Integration:**
- Real-time data streaming via MQTT
- Bidirectional command/control communication
- Dashboard displaying current and historical data (if implemented)
- Alert notification system

**Testing & Documentation:**
- Comprehensive unit test suite (80%+ code coverage target)
- End-to-end integration tests validating full workflows
- 1-hour extended runtime test demonstrating stability
- Complete technical documentation (CDA, GDA, CSF READMEs)
- Final project report with architecture diagrams and analysis

### 5.3 Demonstration Outcomes

**Live Demonstration Capabilities:**
1. Show real-time sensor data collection and display
2. Demonstrate local threshold-based actuation
3. Trigger remote actuation command from cloud/GDA interface
4. Display historical data trends
5. Show LED status indicators responding to conditions
6. Demonstrate profile switching (ale → lager)
7. Present system performance metrics

**Measurable Success Criteria:**
- System runs continuously for 60+ minutes
- Collects 1800+ total sensor readings (30 per metric per hour)
- Triggers 2+ different actuator events
- Maintains target environmental ranges
- Successfully processes remote commands
- All tests pass (unit and integration)
- Complete documentation submitted

---

## 6. Technical Approach

### 6.1 Hardware & Infrastructure

**Edge Device (CDA):**
- Platform: Raspberry Pi 3/4 or equivalent
- OS: Raspbian/Linux
- Language: Python 3.x
- Sensors: SenseHAT emulator or physical sensors (temperature, humidity, pressure)
- Network: WiFi/Ethernet connectivity

**Gateway Device (GDA):**
- Platform: Laptop/desktop or separate Raspberry Pi
- OS: Linux/macOS/Windows
- Language: Java 11+
- Database: Redis for time-series data persistence
- Network: Same LAN as CDA

**Cloud Services:**
- MQTT Broker: HiveMQ Cloud, AWS IoT Core, or similar
- Protocol: MQTT over TLS (port 8883)
- Dashboard: Optional web

## 7. UML Diagram

┌─────────────────────────────────────────────────────────────────┐
│                    FERMENTATION MONITORING SYSTEM               │
│                         CLASS DIAGRAM                           │
└─────────────────────────────────────────────────────────────────┘

CDA (Python) Components:
═══════════════════════════════════════════════════════════════

┌────────────────────────────────┐
│   ConstrainedDeviceApp         │
├────────────────────────────────┤
│ - deviceDataManager            │
├────────────────────────────────┤
│ + startApp()                   │
│ + stopApp()                    │
└────────────────────────────────┘
              │
              │ creates
              ▼
┌─────────────────────────────────┐        ┌──────────────────────┐
│     DeviceDataManager           │◄───────│  ConfigConst         │
├─────────────────────────────────┤        ├──────────────────────┤
│ - currentFermentationProfile    │        │ + ALE_TEMP_MIN       │
│ - currentTempMin                │        │ + ALE_TEMP_MAX       │
│ - currentTempMax                │        │ + LAGER_TEMP_MIN     │
│ - currentHumidityMin            │        │ + LAGER_TEMP_MAX     │
│ - currentHumidityMax            │        │ + HVAC_ACTUATOR_TYPE │
│ - recentTemperatures[]          │        │ + LED_OPTIMAL_CMD    │
│ - currentLedState               │        └──────────────────────┘
│ - actuatorAdapterManager        │
│ - sensorAdapterManager          │
├─────────────────────────────────┤
│ + handleSensorData()            │
│ + handleActuatorCommandMessage()│
│ + _handleFermentationProfile()  │
│ + _detectTemperatureSpike()     │
│ + _handleTemperatureThreshold() │
│ + _handleHumidityThreshold()    │
│ + _activateCooling()            │
│ + _activateHeating()            │
│ + _activateHumidifier()         │
│ + _updateLedDisplay()           │
└─────────────────────────────────┘
         │            │
         │            └──────────────────┐
         │                               │
         ▼                                                   ▼
┌──────────────────────┐    ┌──────────────────────┐
│ SensorAdapterManager │    │ActuatorAdapterManager│
├──────────────────────┤    ├──────────────────────┤
│ - sensorTasks[]      │    │ - actuators[]        │
├──────────────────────┤    ├──────────────────────┤
│ + startManager()     │    │ + sendActuatorCmd()  │
│ + stopManager()      │    │ + setActuatorData()  │
└──────────────────────┘    └──────────────────────┘
         │                               │
         │                               │
    ┌────┴────┬────────┐           ┌─────┴─────┬──────────┐
       ▼              ▼             ▼                  ▼                  ▼                ▼
┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐ ┌────────┐
│TempSen-│ │Humidity│ │Pressure│ │  HVAC  │ │Humidi- │ │  LED   │
│sorTask │ │Sensor  │ │Sensor  │ │Emulator│ │fierEmu-│ │Display │
│        │ │Task    │ │Task    │ │        │ │lator   │ │Emulator│
├────────┤ ├────────┤ ├────────┤ ├────────┤ ├────────┤ ├────────┤
│+genTel-│ │+genTel-│ │+genTel-│ │+update-│ │+update-│ │+update-│
│emetry()│ │emetry()│ │emetry()│ │Actuator│ │Actuator│ │Actuator│
└────────┘ └────────┘ └────────┘ └────────┘ └────────┘ └────────┘


Data Models:
═══════════════════════════════════════════════════════════════

┌─────────────────────┐
│   BaseIotData       │
├─────────────────────┤
│ - name              │
│ - typeID            │
│ - timeStamp         │
│ - statusCode        │
│ - locationID        │
├─────────────────────┤
│ + getName()         │
│ + setName()         │
│ + getTypeID()       │
│ + setTypeID()       │
└─────────────────────┘
          △
          │
    ┌─────┴──────┬──────────────────┐
    │            │                  │
┌───────────┐ ┌──────────────┐ ┌──────────────────────┐
│SensorData │ │ActuatorData  │ │SystemPerformanceData │
├───────────┤ ├──────────────┤ ├──────────────────────┤
│- value    │ │- command     │ │- cpuUtilization      │
│           │ │- value       │ │- memoryUtilization   │
│           │ │- stateData   │ │                      │
├───────────┤ ├──────────────┤ ├──────────────────────┤
│+getValue()│ │+getCommand() │ │+getCpuUtilization()  │
│+setValue()│ │+setCommand() │ │+setCpuUtilization()  │
└───────────┘ │+getStateData()│ └──────────────────────┘
              │+setStateData()|
              └──────────────┘


GDA (Java) Components:
═══════════════════════════════════════════════════════════════

┌────────────────────────────────┐
│    DeviceDataManager (GDA)     │
├────────────────────────────────┤
│ - mqttClient                   │
│ - coapServer                   │
│ - fermentationProfileMgr       │
│ - actuatorDataListener         │
├────────────────────────────────┤
│ + startManager()               │
│ + stopManager()                │
│ + handleSensorMessage()        │
│ + handleActuatorCmdResponse()  │
│ + handleActuatorCmdRequest()   │
│ + changeFermentationProfile()  │
│ - sendActuatorCommandToCda()   │
└────────────────────────────────┘
              │
              │ contains
              ▼
┌────────────────────────────────┐
│  FermentationProfileManager    │
├────────────────────────────────┤
│ - currentProfile               │
│ - currentTempMin               │
│ - currentTempMax               │
│ - currentHumidityMin           │
│ - currentHumidityMax           │
├────────────────────────────────┤
│ + setProfile(name)             │
│ + getCurrentProfile()          │
│ + getCurrentTempMin()          │
│ + getCurrentTempMax()          │
│ + getCurrentHumidityMin()      │
│ + getCurrentHumidityMax()      │
│ + generateProfileChangeCmd()   │
│ + isTemperatureInRange()       │
│ + isHumidityInRange()          │
└────────────────────────────────┘
