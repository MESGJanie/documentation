---
description: Sending commands
---

# Emit an Event

Events are how Core listens to external technologies.

## Communication to the Core

A Service can send Events to the Core. New events are emitted to the Service from its listener function. \(e.g.: a web server running and receiving a request, or a blockchain technology that receives a new transaction\).

* The initial event from the technology

## Event.Emit

[Proto definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go)

{% tabs %}
{% tab title="Request" %}
| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| service | [Service](service-file.md) | Required | Object containing the service definition loaded from the yml service file. |
| event\_id | string | Required | The event's id defined in the [service file](https://github.com/mesg-foundation/documentation/tree/c1028b6f9d709adf2ad46364ce7baaa37e27ff8e/service/service/service-file.md) |
| data | string | Required | The event's data in JSON format |

```javascript
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

```javascript
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



