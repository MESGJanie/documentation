# Listen for events

## Why listen for events?

Your Application needs to listen for events so that it's connected in real time to something happening on a technology. It needs to listen to be able to receive commands. 

Events are emitted to Core from your application's listener function. 

## Listening for events from Services

In order to listen for events from your Services you need to connect to Core through [gRPC](https://grpc.io/) using the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go).

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

#### Example:

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

## Listen for execution outputs

When you execute a task you might want to listen for it's outputs too. Because tasks can take a long time depending on the action they're completing, outputs are sent asynchronously and you need to listen for outputs as a separate process. In order to listen for events you need to connect to Core through [gRPC](https://grpc.io/)  using the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go). 

{% hint style="info" %}
Outputs are send asynchronously. Make sure that you listen for outputs before you try to execute a task, otherwise you might miss the output of this task execution.
{% endhint %}

{% tabs %}
{% tab title="Request" %}
| **Name** | **Type** | **Required** | **Description** |
| --- | --- |
| **service** | [`Service`](../service/service-file.md) | Required | Object that contains the service definition loaded from the yml [service file](../service/service-file.md). |

```javascript
{
  "service": {
    ...
    "tasks": {
      "taskX": {
        "outputs": {
          "outputX": { "type": "String" }
        }
      }
    }
    ...
  }
}
```
{% endtab %}

{% tab title="Stream reply" %}
| Name | Type | Description |
| --- | --- | --- | --- | --- | --- |
| **executionID** | `String` | The ID of the execution of the task that created this output. |
| error | `String` | A string that contains the error if an error is present |
| **taskKey** | `String` | The key of the task as defined in the [service file](../service/service-file.md). |
| **outputKey** | `String` | The key of the output of the task as defined in the [service file](../service/service-file.md). |
| **outputData** | `String` | The data returned by the task serialized in JSON. |

```javascript
{
  "executionID": "xxxxx",
  "error": "",
  "taskKey": "taskX",
  "outputKey": "outputX",
  "outputData": "{\"outputValX\": \"result of execution\"}",
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

