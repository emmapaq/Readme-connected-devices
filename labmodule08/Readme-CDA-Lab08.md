**Constrained Device Application (Connected Devices)**

##Lab Module 08

Description

**What the Implementation does**

Under this Lab module, CoAP server is implemented in CDA which funcions by creating a connection to other IoT devices via the network.  The CoAP server has three resources for accepting actuator instructions from distant devices, and for distributing sensor and system performance data, to which other devices can subscribe for automated updates.  Clients may automatically discover what data is accessible thanks to the server's resource discovery feature.  The server operates in the background without affecting current MQTT and sensor functions, and all communications are in JSON format.

**How it works**

CoapServerAdapter operates an aiocoap server on port 5683 with three handlers: UpdateActuatorResourceHandler receives actuator commands via PUT/POST requests and sends them to DeviceDataManager, while GetSystemPerformanceResourceHandler and GetTelemetryResourceHandler share sensor and performance data with subscribers.  The server enables discovery, arranges resources in a tree structure and operates in a background thread.  Clients receive updates from the handlers when DeviceDataManager automatically provides them with live data.

**Code Repository and Branch**
URL: https://github.com/emmapaq/cda-python-components/tree/labmodule08

**UML Design Diagram(s)**

UML diagram showing CoapServerAdapter managing three handlers (GetSystemPerformanceResourceHandler, GetTelemetryResourceHandler, UpdateActuatorResourceHandler), integrated with DeviceDataManager for data flow between sensors, actuators, and CoAP clients. Includes resource tree structure, JSON conversion via DataUtil, and asyncio threading model.

![Lab Module 07 GDA - UML](Lab_Mod_08_CDA_UML.png)

**Integration Tests Executed**

- ConstrainedDeviceApp 
- CoapServerAdapterTest
