*Constrained Device Application (Connected Devices)**

## Lab Module 06

Description

**What the Implementation does**

In this lab module, implementation adds MQTT messaging to the IoT device app. The device can now connect to an MQTT broker, send sensor data, and receive commands. The MQTT client is created to handles all the network communication and connected it to the main device manager. The system can publish messages with different quality levels and subscribe to topics to get updates.

**How it works**

The MqttClientConnector class handles connecting to the MQTT broker and sending/receiving messages. The DeviceDataManager starts the MQTT client when the app runs and subscribes to actuator command topics. When sensor data is collected, it gets converted to JSON and can be published via MQTT. The system uses configuration files to know which broker to connect to and what topics to use. All the MQTT callback functions log what happens so I can see the communication working.

Code Repository and Branch
URL: https://github.com/emmapaq/cda-python-components/tree/labmodule06

**UML Design Diagram(s)**


**Unit Tests Executed**

- test_MqttClientConnector.py

**Integration Tests Executed**

- test_MqttClientConnector.py
- test_MqttClientControlPacket.py

