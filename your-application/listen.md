# Listen for events

## Why listen for events?

Your Application listens for events so that it is connected in real time to something happening on a technology. Events are emitted to the Core from its listener function. 

## Listening for events

In order to listen for events from your services you need to connect to the core through [gRPC](https://grpc.io/) and using the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go).

{% tabs %}
{% tab title="Request" %}
| **Name** | **Type** | **Required** | **Description** |
| --- | --- |
| service | `Service` | Required | Object that contains the service definition loaded from the yml [service file](../service/service-file.md). |

```javascript
{
  "service": {
    ...
    "events": {
      "eventX": {
        "data": {
          "dataX": { "type": "String" }
        }
      }
    }
    ...
  }
}
```
{% endtab %}

{% tab title="Stream reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- | --- |
| **error** | `String` | A string that contains the error if an error is present |
| **eventKey** | `String` | The event's key defined in the [service file](../service/service-file.md). |
| **eventData** | `String` | The event's data in JSON format |

```javascript
{
  "error": "",
  "eventKey": "eventX",
  "eventData": "{\"dataX\": \"event data\"}"
}
```
{% endtab %}
{% endtabs %}

#### Exemple

{% tabs %}
{% tab title="Node" %}
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
// TODO
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="Go" %}
{% code-tabs %}
{% code-tabs-item title="main.go" %}
```text
// TODO
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

