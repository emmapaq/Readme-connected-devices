**Constrained Device Application (Connected Devices)**

## Lab Module 09

Description

**What the Implementation does**


In my Lab Module 09 implementation, a CoAP client to the CDA was added to facilitace communication with remote IoT servers over the network. This supports all standard CoAP operations: GET requests to retrieve data, POST requests to create new resources, PUT requests to update existing data, and DELETE requests to remove resources. It can discover available resources on remote servers and observe resources for automatic updates when they change. The client works alongside existing MQTT and local sensor functions, and all data exchanges use JSON format.

**How it works**

CoapClientConnector creates an aiocoap client that connects to the GDA CoAP server on port 5683. It sends requests using both CON (confirmable, requiring acknowledgment) and NON (non-confirmable, faster) message types. For observations, the client registers interest in a resource and receives automatic notifications when the resource updates. All incoming responses are processed through callback methods that convert JSON data back into Python objects using DataUtil. The client runs asynchronously using asyncio, allowing it to handle responses without blocking other CDA operations. DeviceDataManager uses the client to communicate sensor data to the GDA and receive actuator commands back.

Code Repository and Branch
URL: https://github.com/emmapaq/cda-python-components/tree/labmodule09

**UML Design Diagram(s)**

UML diagram showing CoapClientConnector with methods for discovery, GET, POST, PUT, DELETE, and observe operations, integrated with DeviceDataManager. Includes async request handlers, response callbacks (_onGetResponse, _onPostResponse, _onPutResponse, _onDeleteResponse), ResourceNameEnum for resource paths, DataUtil for JSON conversion, and asyncio event loop for non-blocking operations.

![Lab Module 09 CDA - UML](lab_mod_09_CDA.png)


**Integration Tests Executed**

1.test_CoapClientConnector
