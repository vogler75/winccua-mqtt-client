# MQTT Client for WinCC Unified
It is a Custom Web Control for SIEMENS WinCC Unified to connect to MQTT brokers.

! You have to download the webcc.min.js from SIEMENS and save it to the control/js directory !  

Create a Custom-Web.Control zip file from this directory:   
> cd winccua-mqtt-client\Custom-Web-Control\{9117B3FF-7043-5BE4-B472-EA44D2488BF8}

> "C:\Program Files\7-Zip\7z.exe" a -tzip "..\{9117B3FF-7043-5BE4-B472-EA44D2488BF8}.zip" *

Copy the {9117B3FF-7043-5BE4-B472-EA44D2488BF8}.zip file to your project into the UserFiles\CustomControls directory.

Then you can drag and drop it to your screen and configure it.

The [Eclipse Paho JavaScript Client](https://eclipse.dev/paho/index.php?page=clients/js/index.php) is used in this Custom-Web-Control. 

## Properties
* BrokerUrl: Url to your MQTT Broker, for example ws://192.168.1.1:9001/mqtt
* ClientId: MQTT Client Id
* Connect: Connect to MQTT Broker if it is set to true
* Debug: Some debug messages will be written to the browser console if set to true
* Topics: List of topics to which you want to subscribe. Example: ["test/+/value", "motor/#"]
* UseSSL: SSL connection will be used if set to true

## Events
* onConnect: fired when the connection is created.
* onConnectionLost: fired when the connection got lost
* onMessageArrived: fired when a message comes in. 

Here is an example for the script on the onMessageArrived event.
```
export function MQTT_1_onMessageArrived(item, message) {
  Screen.Items("Message").Text = message;
  let data = JSON.parse(message);
  switch (data.topic) {
    case "test/gauge/1": Screen.Items("Gauge_1").ProcessValue = data.payload; break;
    case "test/gauge/2": Screen.Items("Gauge_2").ProcessValue = data.payload; break;
  }
}
```

The Message JSON-String looks like this:
```
{
    "topic": "test/hello",
    "payload": "Hello World!",
    "qos": 0,
    "retained": false,
    "duplicate": false
}
```

## Method

There is a method "Publish" to publish values to the MQTT broker. It has one object as argument, which must contain "topic" and "payload". Optionally "qos" and "retained" can also be in the object. 

Example
```
Screen.Items("MQTT_1").Publish({
    "topic": "test/hello", 
    "payload": "Hello World!",
    "qos": 0,
    "retained": false
});
```