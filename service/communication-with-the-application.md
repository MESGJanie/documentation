# Communication with the Core

## Send events to the Core

The Service can send data to the Application. The data is divided into three categories:

* Result of a command: When the Application asks to start a command and expects a result from this command
* Events from listener: When the Service emits a new event from its listener function. \(Eg: web server running and receiving a request or a blockchain technology that received a new transaction\)
* System events: When the service is ready to be executed, notify the Application

## Event.Emit

[Proto definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go)

{% tabs %}
{% tab title="Request" %}
```javascript
{
    "service": {...},
    "event": "ethereum_newBlock",
    "data": "{\"number\":2323232}"
}
```

#### Example

#### Parameters

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| service | [Service](service-file.md) | Required | Object containing the service definition loaded from the yml service file. |
| event | string | Required | The name of the event |
| data | string | Required | The JSON Stringify of the event's data |
{% endtab %}

{% tab title="Reply" %}
#### Example

```javascript
{
    "service": {...},
    "event": "ethereum_newBlock",
    "data": "{\"number\":2323232}"
}
```

#### Parameters

| **Name** | **Type** | **Description** |
| --- | --- | --- |
| event | string |  |
| data | string |  |
{% endtab %}

{% tab title="Example NodeJS" %}
```javascript
const grpc = require('grpc')
const types = grpc.load(__dirname + '/types/api_event.proto').types
const eventClient = new types.Event('localhost:50052', grpc.credentials.createInsecure())

const service = "" //TODO: read service file
const eventData = {} //TODO: add event data

eventClient.Emit({
  service: service,
  event: "event_newBlock",
  data: JSON.stringify(eventData)
}, (err, reply) => {
  if (err) {
    console.error('err', err)
    return
  }
  console.log('core reply', reply)
})
```
{% endtab %}
{% endtabs %}







## Receiving commands from the Core

The Service needs to receive the command sent by the Application. Every time a command is received, it will make sure that the sender is the Application, then check that it can handle the command, and if so, execute it. Once executed, it will emit an event to the Application with the result of the command.

**TODO: a technical definition**

