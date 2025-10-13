# Constrained Device Application (Connected Devices)

## Lab Module 04 - Data Emulation

Lab Module 04 - Data Emulation is focused on building the components that will work with a software emulator (Sense HAT Emulator) to dynamically generate time-series sensor data, such as temperature, humidity and pressure. This will also introduce the concept of emulated actuation, where the Sense HAT Emulator's LED display can be activated via an actuation command.

### Description

What does your implementation do? 
My implementation creates Python modules to simulate sensor and actuator tasks using the Pisense and Sense-Emu libraries. 
These modules mimic real-world IoT devices like temperature, humidity, and pressure sensors, as well as actuators, by providing emulated functionality. 
The emulator allows these sensor and actuator tasks to be performed without physical hardware. Additionally, the implementation updates the SensorAdapterManager and ActuatorAdapterManager modules to handle the interaction between the emulated sensors/actuators and the system using the SenseHAT emulator.

How does your implementation work?
The implementation works by inheriting from base classes, BaseSensorSimTask and BaseActuatorSimTask, which provide foundational functionality for sensor and actuator tasks. 
The emulator modules are structured to follow the design of their simulator counterparts but with additional detailed implementations for sensor and actuator emulation. 
The SenseHAT emulator is integrated through the SensorAdapterManager and ActuatorAdapterManager classes, which coordinate the flow of sensor data and actuator commands in the simulated environment. 
The Pisense and Sense-Emu libraries enable the simulation of sensors, allowing for testing IoT systems in a controlled, virtual environment that mimics real-world device behavior.

### Code Repository and Branch

URL: https://github.com/emmapaq/python-components/tree/labmodule04

### UML Design Diagram(s)

![Lab Module 04 UML](LabModule04_UML.png)

![Lab Module 04 UML](LabModule04_UML_02.png)

### Unit Tests Executed
- test_SenseHatEmulatorQuickTest


### Integration Tests Executed

- test_HumidityEmulatorTask.py
- test_PressureEmulatorTask.py
- test_TemperatureEmulatorTask.py

- test_SensorEmulatorManager
- test_ActuatorEmulatorManager
- test_EmbeddedSensorAdapter


