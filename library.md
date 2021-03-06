# att\_iot\_client.ATT_IOT Module


## Data
- `ClientId = None`   
The clientId to authenticate the connection. This can be found on the website. 
- `ClientKey = None`  
The clientKey to authenticate the connection. This can be found on the website. 
- `DeviceId = None`  
The device has to be created on the website. Assign the id of the device to this variable. 
- `on_message = None`   
The callback function for processing actuator commands. The signatue of the callback function should be:  
```Python
def on_message(id, value)
``` 

## Functions

### addAsset 

```Python
addAsset(id, name, description, isActuator, assetType, style='Undefined')
``` 

Create or update the specified asset. Call this function after calling 'connect' for each asset that you want to use.

_parameters:_

- `id:` the local id of the asset
<type>type id:</type> string or number
- `name:` the label that should be used to show on the website
<type>type name:</type> basestring
- `description:` a description of the asset
<type>type description:</type> basestring
- `isActuator:` True if this is an actuator. When False, it's created as a Sensor
<type>type isActuator:</type> boolean
- `assetType:` the type of the asset, possible values: 'integer', 'number', 'boolean', 'text', None (defaults to string, when the asset already exists, the website will not overwrite any changes done manually on the site). Can also be a complete profile definition as a json string (see http://allthingstalk.com/docs/maker/concepts/assets/profiles/) example: '{"type": "integer", "minimum": 0}'.
<type>type assetType:</type> string
- `style:` possible values: 'Primary', 'Secondary', 'Config', 'Battery'
<type>type style:</type> basestring 

### closeHttp 

```Python
closeHttp()
``` 

closes the http connection, if it was open


_returns_: None 

### closeMqtt 

```Python
closeMqtt()
``` 

closes the mqtt connection, if opened.


_returns_: None 

### connect 

```Python
connect(httpServer='api.allthingstalk.io', secure=False)
``` 

create an HTTP connection with the server

_parameters:_

- `httpServer:` The dns name of the server to use for HTTP communication
- `secure:` When true, an SSL connection will be used, if available. 

### deleteDevice 

```Python
deleteDevice()
``` 

Deletes the currently loaded device from the cloud.  After this function, the global DeviceId will be reset to None 

### getAssetState 

```Python
getAssetState(asset, device=None)
``` 

Gets the last recorded value for the specified asset.
When device is ommitted (or None), the current device is used, otherwise the device with the specified id is used.

_parameters:_

- `device:` The id of the device to use. When None, the current device is queried.
- `asset:` The id of the d
<type>type asset:</type> string or int


_returns_ a json object containing the last recorded data. 

### getAssets 

```Python
getAssets()
``` 

gets the list of assets that are known for this device in the cloud


_returns_ a json array containing all the assets. 

### getPrimaryAsset 

```Python
getPrimaryAsset()
``` 

returns,as a list, the asset(s) assigned to the device as being "primary", that is, these assets represent the main functionality
of the device. Ex: a wall plug - powerswithch  can have many assets, but it's primary function is to switch on-off
		return type: A json array contains all the assets that the cloud knows of for the current device and which have been labeled to be primary 

### send 

```Python
send(value, assetId)
``` 

Use this function to send a data value to the cloud server, using MQTT, for the asset with the specified id as provided by the IoT platform.

_parameters:_

- `value:` the value to send. This can be in the form of a string, int, double, bool or python object/list All primitive values are converted to a lower case string, ex: 'true' or 'false'
You can also send an object or a python list with this function to the cloud. Objects will be converted to json objects, lists become json arrays. The fields/records in the json objects or arrays must be the same as defined in the profile.
<type>type value:</type> number, string, boolean, object or list
- `assetId:` the id of the asset to send the value to, usually the pin number. This is the local id that you used while creating/updating the asset through the function 'addAsset' ex: 1
<type>type assetId:</type> string or number 

### sendCommandTo 

```Python
sendCommandTo(value, device, actuator)
``` 

send a command to the specified actuator. The device has to be in the same account as this device)

_parameters:_

- `value:` the value to send
- `device:` the device id to send it to.
- `actuator:` the local id of the actuator (name) 

### sendCommandToHTTP 

```Python
sendCommandToHTTP(value, assetId)
``` 

Sends a command to an asset on another device using http(s).
The assetId should be the full id (string), as seen on the cloud app.
Note: you can only send commands to actuators that belong to devices in the same account as this device.

ex: sendCommandTo('122434545abc112', 1)


_parameters:_

- `value:` same as for 'send' and 'sendValueHTTP'
- `assetId:` the id of the asset to send the value to. This id must be the full id as found on the cloud app
<type>type assetId:</type> basestring 

### sendValueHTTP 

```Python
sendValueHTTP(value, assetId)
``` 

Sends a new value for an asset over http. This function is similar to send, accept that the latter uses mqtt
while this function uses HTTP

Parameters are the same as for the send function. 

### subscribe 

```Python
subscribe(mqttServer='api.allthingstalk.io', port=1883, secure=False, certFile='cacert.pem')
``` 

Sets up everything for the pub-sub client: create the connection, provide the credentials and register for any possible incoming data.
This function also closes the http connection if it was opened.
If you need both http and mqtt opened at the same time, it's best to open mqtt first, then open the http connection.

_parameters:_

- `mqttServer:`  the address of the mqtt server. Only supply this value if you want to a none standard server. Default = api.AllThingsTalk.io
- `port:` the port number to communicate on with the mqtt server. Default = 1883
- `secure:` When true, an SSL connection is used. Default = False.  When True, use port 8883 on api.AllThingsTalk.io
- `certFile:` certfile is a string pointing to the PEM encoded client
certificate and private keys respectively. Note
that if either of these files in encrypted and needs a password to
decrypt it, Python will ask for the password at the command line. It is
not currently possible to define a callback to provide the password.
Note: SSL will can only be used when the mqtt lib has been compiled with support for ssl. At the moment, SSL is only
available on the broker.smartliving.io endppoint, due to a restriction in the paho mqtt library.
SSL on api.allthingstalk.io will become available as soon as the paho mqtt library has been updated. 

### updateDevice 

```Python
updateDevice(name, description, activityEnabled=False)
``` 

updates the definition of the device

_parameters:_

- `name:` The name of the device
- `description:` the description for the device
- `activityEnabled:` When True, historical data will be stored on the db, otherwise only the last received value is stored. 

# att\_iot\_client.nw_watchdog Module

This module can be used to verify the mqtt network connection and perform the required actions in case that the connection has been lost.

## Data
- `PingFrequency = 3000`  
The time interval between consecutive pings that the watchdog sends out, expressed in milli seconds. The default values is 3 seconds.
- `WatchDogAssetId = -1`  
The id of the watchdog asset that will be used. By default, this is -1.
- `on_failure = None`   
A reference to a function that will be called when a network failure has been detected. This allows you to perform any custom actions, like rebooting the device. It's signature should be:  
```Python
def on_failure()
``` 

## Functions

### checkPing 

```Python
checkPing()
``` 

check if we need to resend a ping and if we received the previous ping in time 

### isWatchDog 

```Python
isWatchDog(id, value)
``` 

check if an incomming command was a ping from the watchdog


_returns_: True if the id was that of the watchdog.
		return type: bool 

### ping 

```Python
ping()
``` 

send a ping to the server. 

### setup 

```Python
setup()
``` 

Add the watchdog asset to the device.  This can be used as visual feedback for the state of the device. 