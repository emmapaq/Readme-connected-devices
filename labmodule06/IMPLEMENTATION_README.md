# MQTT Integration Implementation Guide

## Complete Project Structure

Here's the complete directory structure for your CDA Python components:

```
cda-python-components/
├── config/
│   └── PiotConfig.props
│
├── programmingtheiot/
│   ├── __init__.py
│   │
│   ├── common/
│   │   ├── __init__.py
│   │   ├── ConfigConst.py               # Configuration constants
│   │   ├── ConfigUtil.py                # Configuration utility (you need to create)
│   │   ├── ResourceNameEnum.py          # Resource name enumerations
│   │   ├── IDataMessageListener.py      # Interface (you need to create)
│   │   └── DefaultDataMessageListener.py # Default listener (you need to create)
│   │
│   ├── cda/
│   │   ├── __init__.py
│   │   │
│   │   ├── app/
│   │   │   ├── __init__.py
│   │   │   ├── ConstrainedDeviceApp.py  # Main application
│   │   │   └── DeviceDataManager.py     # Device data manager
│   │   │
│   │   ├── connection/
│   │   │   ├── __init__.py
│   │   │   └── MqttClientConnector.py   # MQTT client (you need to create)
│   │   │
│   │   └── system/
│   │       ├── __init__.py
│   │       ├── SystemPerformanceManager.py    # Optional
│   │       ├── SensorAdapterManager.py        # Optional
│   │       └── ActuatorAdapterManager.py      # Optional
│   │
│   └── data/
│       ├── __init__.py
│       ├── BaseIotData.py              # Base data class
│       ├── ActuatorData.py             # Actuator data
│       ├── SensorData.py               # Sensor data
│       ├── SystemPerformanceData.py    # System performance data
│       └── DataUtil.py                 # JSON conversion utility (you need to create)
│
└── tests/
    └── integration/
        └── connection/
            ├── __init__.py
            ├── test_MqttClientConnector.py        # Integration tests
            └── test_MqttClientControlPacket.py    # Control packet tests
```

## Files Created in This Implementation

### Core Application Files
1. **ConfigConst.py** - All configuration constants and defaults
2. **DeviceDataManager.py** - Main device data manager with MQTT integration
3. **ConstrainedDeviceApp.py** - Application entry point
4. **BaseIotData.py** - Base class for all IoT data types
5. **ActuatorData.py** - Actuator data with command constants
6. **SensorData.py** - Sensor data representation
7. **SystemPerformanceData.py** - System metrics data
8. **ResourceNameEnum.py** - Resource name enumerations

### Test Files
9. **test_MqttClientConnector.py** - Integration tests
10. **test_MqttClientControlPacket.py** - All 14 MQTT control packets test

### Configuration
11. **PiotConfig.props** - Configuration file template
12. **SETUP_AND_RUN_GUIDE.md** - Complete setup and testing guide

## Files You Need to Create

You'll need to implement these additional files based on your existing codebase or create stubs:

### 1. ConfigUtil.py
```python
"""Configuration utility for reading PiotConfig.props"""
import configparser
import os

class ConfigUtil:
    def __init__(self, configFile='./config/PiotConfig.props'):
        self.config = configparser.ConfigParser()
        if os.path.exists(configFile):
            self.config.read(configFile)
    
    def getProperty(self, section, key, defaultVal=None):
        try:
            return self.config.get(section, key)
        except:
            return defaultVal
    
    def getInteger(self, section, key, defaultVal=0):
        try:
            return self.config.getint(section, key)
        except:
            return defaultVal
    
    def getFloat(self, section, key, defaultVal=0.0):
        try:
            return self.config.getfloat(section, key)
        except:
            return defaultVal
    
    def getBoolean(self, section, key, defaultVal=False):
        try:
            return self.config.getboolean(section, key)
        except:
            return defaultVal
```

### 2. IDataMessageListener.py
```python
"""Interface for data message listeners"""
from abc import ABC, abstractmethod

class IDataMessageListener(ABC):
    @abstractmethod
    def handleActuatorCommandRequest(self, data):
        pass
    
    @abstractmethod
    def handleActuatorCommandResponse(self, data):
        pass
    
    @abstractmethod
    def handleIncomingMessage(self, resourceEnum, msg):
        pass
    
    @abstractmethod
    def handleSensorMessage(self, data):
        pass
    
    @abstractmethod
    def handleSystemPerformanceMessage(self, data):
        pass
```

### 3. DefaultDataMessageListener.py
```python
"""Default implementation of data message listener"""
import logging
from programmingtheiot.common.IDataMessageListener import IDataMessageListener

class DefaultDataMessageListener(IDataMessageListener):
    def handleActuatorCommandRequest(self, data):
        logging.info(f"Actuator command request: {data}")
        return None
    
    def handleActuatorCommandResponse(self, data):
        logging.info(f"Actuator command response: {data}")
        return True
    
    def handleIncomingMessage(self, resourceEnum, msg):
        logging.info(f"Incoming message on {resourceEnum}: {msg}")
        return True
    
    def handleSensorMessage(self, data):
        logging.info(f"Sensor message: {data}")
        return True
    
    def handleSystemPerformanceMessage(self, data):
        logging.info(f"System performance message: {data}")
        return True
```

### 4. DataUtil.py
```python
"""Utility for converting data objects to/from JSON"""
import json
import logging
from datetime import datetime

class DataUtil:
    def __init__(self):
        logging.info("Created DataUtil instance.")
    
    def actuatorDataToJson(self, data):
        """Convert ActuatorData to JSON string"""
        if not data:
            return None
        
        jsonData = {
            'timeStamp': data.getTimeStamp(),
            'hasError': data.hasErrorFlag(),
            'name': data.getName(),
            'typeID': data.getTypeID(),
            'statusCode': data.getStatusCode(),
            'latitude': data.getLatitude(),
            'longitude': data.getLongitude(),
            'elevation': data.getElevation(),
            'locationID': data.getLocationID(),
            'value': data.getValue(),
            'command': data.getCommand(),
            'stateData': data.getStateData()
        }
        
        return json.dumps(jsonData, indent=4)
    
    def jsonToActuatorData(self, jsonStr):
        """Convert JSON string to ActuatorData"""
        from programmingtheiot.data.ActuatorData import ActuatorData
        
        if not jsonStr:
            return None
        
        jsonData = json.loads(jsonStr)
        data = ActuatorData()
        
        data.setName(jsonData.get('name', 'Not Set'))
        data.setTypeID(jsonData.get('typeID', 0))
        data.setStatusCode(jsonData.get('statusCode', 0))
        data.setValue(jsonData.get('value', 0.0))
        data.setCommand(jsonData.get('command', 0))
        data.setStateData(jsonData.get('stateData', ''))
        
        return data
    
    def sensorDataToJson(self, data):
        """Convert SensorData to JSON string"""
        if not data:
            return None
        
        jsonData = {
            'timeStamp': data.getTimeStamp(),
            'hasError': data.hasErrorFlag(),
            'name': data.getName(),
            'typeID': data.getTypeID(),
            'statusCode': data.getStatusCode(),
            'latitude': data.getLatitude(),
            'longitude': data.getLongitude(),
            'elevation': data.getElevation(),
            'locationID': data.getLocationID(),
            'value': data.getValue()
        }
        
        return json.dumps(jsonData, indent=4)
    
    def systemPerformanceDataToJson(self, data):
        """Convert SystemPerformanceData to JSON string"""
        if not data:
            return None
        
        jsonData = {
            'timeStamp': data.getTimeStamp(),
            'hasError': data.hasErrorFlag(),
            'name': data.getName(),
            'typeID': data.getTypeID(),
            'statusCode': data.getStatusCode(),
            'cpuUtil': data.getCpuUtilization(),
            'memUtil': data.getMemoryUtilization(),
            'diskUtil': data.getDiskUtilization()
        }
        
        return json.dumps(jsonData, indent=4)
```

### 5. MqttClientConnector.py (Stub/Template)
```python
"""MQTT Client Connector for CDA"""
import logging
import paho.mqtt.client as mqtt

from programmingtheiot.common.ConfigUtil import ConfigUtil
from programmingtheiot.common.ResourceNameEnum import ResourceNameEnum
import programmingtheiot.common.ConfigConst as ConfigConst

class MqttClientConnector:
    def __init__(self, clientID=None):
        self.config = ConfigUtil()
        
        # Get MQTT configuration
        self.host = self.config.getProperty(
            ConfigConst.MQTT_GATEWAY_SERVICE,
            ConfigConst.HOST_KEY,
            ConfigConst.DEFAULT_HOST
        )
        self.port = self.config.getInteger(
            ConfigConst.MQTT_GATEWAY_SERVICE,
            ConfigConst.PORT_KEY,
            ConfigConst.DEFAULT_MQTT_PORT
        )
        self.keepAlive = self.config.getInteger(
            ConfigConst.MQTT_GATEWAY_SERVICE,
            ConfigConst.KEEP_ALIVE_KEY,
            ConfigConst.DEFAULT_KEEP_ALIVE
        )
        
        if not clientID:
            clientID = self.config.getProperty(
                ConfigConst.MQTT_GATEWAY_SERVICE,
                ConfigConst.CLIENT_ID_KEY,
                "CDAMqttClient"
            )
        
        self.clientID = clientID
        self.mqttClient = mqtt.Client(client_id=self.clientID)
        
        # Set callbacks
        self.mqttClient.on_connect = self._onConnect
        self.mqttClient.on_disconnect = self._onDisconnect
        self.mqttClient.on_message = self._onMessage
        self.mqttClient.on_publish = self._onPublish
        
        self.listener = None
        
        logging.info(f"\tMQTT Client ID:   {self.clientID}")
        logging.info(f"\tMQTT Broker Host: {self.host}")
        logging.info(f"\tMQTT Broker Port: {self.port}")
        logging.info(f"\tMQTT Keep Alive:  {self.keepAlive}")
    
    def connectClient(self):
        """Connect to MQTT broker"""
        try:
            logging.info(f"MQTT client connecting to broker at host: {self.host}")
            self.mqttClient.connect(self.host, self.port, self.keepAlive)
            self.mqttClient.loop_start()
            return True
        except Exception as e:
            logging.error(f"Failed to connect MQTT client: {e}")
            return False
    
    def disconnectClient(self):
        """Disconnect from MQTT broker"""
        try:
            logging.info(f"Disconnecting MQTT client from broker: {self.host}")
            self.mqttClient.loop_stop()
            self.mqttClient.disconnect()
            return True
        except Exception as e:
            logging.error(f"Failed to disconnect MQTT client: {e}")
            return False
    
    def publishMessage(self, resource, msg, qos=0):
        """Publish message to topic"""
        try:
            topic = resource.value if isinstance(resource, ResourceNameEnum) else str(resource)
            self.mqttClient.publish(topic, msg, qos)
            return True
        except Exception as e:
            logging.error(f"Failed to publish message: {e}")
            return False
    
    def subscribeToTopic(self, resource, callback=None, qos=0):
        """Subscribe to topic"""
        try:
            topic = resource.value if isinstance(resource, ResourceNameEnum) else str(resource)
            self.mqttClient.subscribe(topic, qos)
            logging.info(f"Subscribed to topic: {topic}")
            return True
        except Exception as e:
            logging.error(f"Failed to subscribe to topic: {e}")
            return False
    
    def unsubscribeFromTopic(self, resource):
        """Unsubscribe from topic"""
        try:
            topic = resource.value if isinstance(resource, ResourceNameEnum) else str(resource)
            self.mqttClient.unsubscribe(topic)
            logging.info(f"Unsubscribed from topic: {topic}")
            return True
        except Exception as e:
            logging.error(f"Failed to unsubscribe from topic: {e}")
            return False
    
    def setDataMessageListener(self, listener):
        """Set the data message listener"""
        self.listener = listener
    
    def _onConnect(self, client, userdata, flags, rc):
        """Callback for connection"""
        logging.info(f"MQTT client connected to broker: {client}")
    
    def _onDisconnect(self, client, userdata, rc):
        """Callback for disconnection"""
        logging.info(f"MQTT client disconnected from broker: {client}")
    
    def _onMessage(self, client, userdata, msg):
        """Callback for received message"""
        logging.info(f"MQTT message received with payload: {msg.payload.decode('utf-8')}")
        
        if self.listener:
            # Find matching resource enum
            for resource in ResourceNameEnum:
                if resource.value == msg.topic:
                    self.listener.handleIncomingMessage(resource, msg.payload.decode('utf-8'))
                    break
    
    def _onPublish(self, client, userdata, mid):
        """Callback for published message"""
        logging.info(f"MQTT message published: {client}")
```

## Creating __init__.py Files

Every Python package directory needs an `__init__.py` file (can be empty):

```bash
# Create all __init__.py files
touch programmingtheiot/__init__.py
touch programmingtheiot/common/__init__.py
touch programmingtheiot/cda/__init__.py
touch programmingtheiot/cda/app/__init__.py
touch programmingtheiot/cda/connection/__init__.py
touch programmingtheiot/cda/system/__init__.py
touch programmingtheiot/data/__init__.py
touch tests/__init__.py
touch tests/integration/__init__.py
touch tests/integration/connection/__init__.py
```

## Installation Steps

### 1. Create Directory Structure
```bash
mkdir -p cda-python-components/programmingtheiot/{common,cda/{app,connection,system},data}
mkdir -p cda-python-components/tests/integration/connection
mkdir -p cda-python-components/config
```

### 2. Copy Files to Correct Locations

Place each artifact file in its correct location:

- `ConfigConst.py` → `programmingtheiot/common/`
- `ResourceNameEnum.py` → `programmingtheiot/common/`
- `BaseIotData.py` → `programmingtheiot/data/`
- `ActuatorData.py` → `programmingtheiot/data/`
- `SensorData.py` → `programmingtheiot/data/`
- `SystemPerformanceData.py` → `programmingtheiot/data/`
- `DeviceDataManager.py` → `programmingtheiot/cda/app/`
- `ConstrainedDeviceApp.py` → `programmingtheiot/cda/app/`
- `test_MqttClientConnector.py` → `tests/integration/connection/`
- `test_MqttClientControlPacket.py` → `tests/integration/connection/`
- `PiotConfig.props` → `config/`

### 3. Create Missing Files

Create the following files using the templates above:
- `ConfigUtil.py` in `programmingtheiot/common/`
- `IDataMessageListener.py` in `programmingtheiot/common/`
- `DefaultDataMessageListener.py` in `programmingtheiot/common/`
- `DataUtil.py` in `programmingtheiot/data/`
- `MqttClientConnector.py` in `programmingtheiot/cda/connection/`

### 4. Create All __init__.py Files

```bash
cd cda-python-components
touch programmingtheiot/__init__.py
touch programmingtheiot/common/__init__.py
touch programmingtheiot/cda/__init__.py
touch programmingtheiot/cda/app/__init__.py
touch programmingtheiot/cda/connection/__init__.py
touch programmingtheiot/cda/system/__init__.py
touch programmingtheiot/data/__init__.py
touch tests/__init__.py
touch tests/integration/__init__.py
touch tests/integration/connection/__init__.py
```

### 5. Install Dependencies

```bash
pip install paho-mqtt
pip install apscheduler
pip install pytest  # For running tests
```

### 6. Set PYTHONPATH

```bash
# From project root (cda-python-components)
export PYTHONPATH="${PYTHONPATH}:$(pwd)/programmingtheiot"
```

## Resolving Import Errors

### Error: "Undefined variable from import"

This typically means the constant is defined in ConfigConst.py but your IDE hasn't indexed it yet.

**Solution:**
1. Verify `ConfigConst.py` contains the constants:
   - `POLLING_CYCLES_KEY`
   - `DEFAULT_POLLING_CYCLES`
   - `COMMAND_ON`
   - `HVAC_ACTUATOR_TYPE`

2. Refresh your IDE's project structure
3. Rebuild the project index

### Error: "Unresolved import: DeviceDataManager"

This means the import path is incorrect.

**Solution:**
Make sure `DeviceDataManager.py` is in `programmingtheiot/cda/app/` not `programmingtheiot/cda/system/`

The correct import is:
```python
from programmingtheiot.cda.app.DeviceDataManager import DeviceDataManager
```

### Error: "Encountered <INDENT> at line 2"

This is a syntax error in your Python file.

**Solution:**
1. Check the file for invalid indentation
2. Make sure the file doesn't start with unexpected whitespace
3. Verify all function/class definitions are properly structured

## Testing Your Setup

### 1. Verify Project Structure
```bash
tree cda-python-components
```

Should show the structure outlined at the beginning.

### 2. Test Imports
```bash
cd cda-python-components
python3 -c "import programmingtheiot.common.ConfigConst as ConfigConst; print(ConfigConst.COMMAND_ON)"
```

Should print: `1`

### 3. Test Configuration
```bash
python3 -c "from programmingtheiot.common.ConfigUtil import ConfigUtil; cfg = ConfigUtil(); print('Config loaded')"
```

### 4. Run Application
```bash
python programmingtheiot/cda/app/ConstrainedDeviceApp.py
```

## Common Issues and Solutions

### Issue 1: ModuleNotFoundError

**Error:**
```
ModuleNotFoundError: No module named 'programmingtheiot'
```

**Solution:**
```bash
# Make sure you're in the project root
cd cda-python-components

# Set PYTHONPATH
export PYTHONPATH="${PYTHONPATH}:$(pwd)"

# Verify
echo $PYTHONPATH
```

### Issue 2: Import Cycles

**Error:**
```
ImportError: cannot import name 'X' from partially initialized module
```

**Solution:**
- Check for circular imports
- Move imports inside functions if necessary
- Use `import module` instead of `from module import class`

### Issue 3: Paho MQTT Version Issues

**Error:**
```
AttributeError: 'Client' object has no attribute 'connected_flag'
```

**Solution:**
```bash
pip install --upgrade paho-mqtt
```

### Issue 4: Configuration File Not Found

**Error:**
```
FileNotFoundError: config/PiotConfig.props
```

**Solution:**
```bash
# Create config directory
mkdir -p config

# Copy template
cp PiotConfig.props.template config/PiotConfig.props

# Or update ConfigUtil to use correct path
# Edit ConfigUtil.py and update the default path
```

## Running Tests

### Run All Tests
```bash
cd cda-python-components
python -m pytest tests/integration/connection/ -v
```

### Run Specific Test
```bash
# Integration test
python -m pytest tests/integration/connection/test_MqttClientConnector.py::MqttClientConnectorTest::testActuatorCmdPubSub -v

# Control packet test
python -m pytest tests/integration/connection/test_MqttClientControlPacket.py::MqttClientControlPacketTest::testAllControlPackets -v
```

### Run Tests Directly
```bash
python tests/integration/connection/test_MqttClientConnector.py
python tests/integration/connection/test_MqttClientControlPacket.py
```

## Verification Checklist

- [ ] All directories created
- [ ] All `__init__.py` files created
- [ ] All artifact files copied to correct locations
- [ ] Missing files created (ConfigUtil, DataUtil, MqttClientConnector, etc.)
- [ ] PYTHONPATH set correctly
- [ ] Dependencies installed (paho-mqtt, apscheduler)
- [ ] MQTT broker installed and running
- [ ] Configuration file created and updated
- [ ] Can import ConfigConst successfully
- [ ] Can import DeviceDataManager successfully
- [ ] Can run ConstrainedDeviceApp.py
- [ ] Integration tests pass

## Next Steps

1. **Start MQTT Broker:**
   ```bash
   sudo systemctl start mosquitto
   ```

2. **Run CDA Application:**
   ```bash
   python programmingtheiot/cda/app/ConstrainedDeviceApp.py
   ```

3. **Run Integration Tests:**
   ```bash
   python tests/integration/connection/test_MqttClientConnector.py
   ```

4. **Verify All 14 Control Packets:**
   ```bash
   python tests/integration/connection/test_MqttClientControlPacket.py
   ```

## Support Resources

- **MQTT Protocol:** http://docs.oasis-open.org/mqtt/mqtt/v3.1.1/mqtt-v3.1.1.html
- **Paho MQTT Python:** https://www.eclipse.org/paho/index.php?page=clients/python/docs/index.php
- **Python Packaging:** https://packaging.python.org/tutorials/packaging-projects/

---

**Last Updated:** October 2024
**Version:** 1.0
**License:** PIOT-DOC-LIC
