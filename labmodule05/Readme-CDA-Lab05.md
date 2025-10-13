**Constrained Device Application (Connected Devices)**

## Lab Module 05 - Data Management

Description

**What the Implementation does**

Lab Module 5 builds a system monitoring and data handling system for IoT devices. It tracks CPU and memory usage through SystemPerformanceManager and converts data between Python objects and JSON format. The system collects real performance metrics and sends them to registered listeners through callback functions. DataUtil handles JSON conversion for sensor data, actuator data, and performance data.

**How it works**

SystemPerformanceManager runs on a schedule to collect CPU and memory usage from the system. It stores this data in SystemPerformanceData objects and calls registered listeners when new data is ready. DataUtil uses Python's json library to convert objects like sensor readings and actuator commands into JSON format and back to Python objects. The system uses callback patterns so different components can react when new data arrives. All components connect through the IDataMessageListener interface for consistent data flow.

Code Repository and Branch
URL: https://github.com/emmapaq/cda-python-components/tree/labmodule05

**UML Design Diagram(s)**

UML diagram indicates SystemPerformanceManager collecting system metrics with callback support, DataUtil handling JSON conversion, SystemPerformanceData storing performance information, and IDataMessageListener connecting system components.

![Lab Module 05 CDA - UML](Lab05_CDA_UML.jpg)

**Unit Tests Executed**

1. test_DataUtil 

**Integration Tests Executed**

1. test_SystemPerformanceManager
2. test_DataIntegrationTest
