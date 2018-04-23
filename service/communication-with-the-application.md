# Communication with the Core

Add service life cycle: wait for sync before listening for task. 

## Send events to the Core

The Service can send data to the Core. The data is divided into three categories:

* Events from listener: When the Service emits a new event from its listener function. \(Eg: web server running and receiving a request or a blockchain technology that received a new transaction\)

## Event.Emit

[Proto definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go)

{% tabs %}
{% tab title="Request" %}
| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| service | [Service](service-file.md) | Required | Object containing the service definition loaded from the yml service file. |
| event_id | string | Required | The event's id defined in the [service file](service/service-file.md) |
| data | string | Required | The event's data in JSON format |

```json
{
    "service": {
      ...
      "events": {
        "eventX": {
          "data": {
            "foo": {
              "type": "string"
            }
          }
        }
      },
      ...
    },
    "event_id": "eventX",
    "data": "{\"foo\":\"bar\"}"
}
```
{% endtab %}

{% tab title="Reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- |
| event | string |  |
| data | string |  |

```json
{
    "service": {...},
    "event": "ethereum_newBlock",
    "data": "{\"number\":2323232}"
}
```
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

The Service needs to receive the command sent by the Core. Every time a command is received, it will make sure that the sender is the Core, then check that it can handle the command, and if so, execute it. Once executed, it will reply to the Core with the result of the command.

**TODO: a technical definition**

