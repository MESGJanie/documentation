# Execute a task

## Why execute a task?

Your Application executes tasks to feed information in real time to the MESG ecosystem or to trigger a task on any other technology that is connected to your Application. 

## Execute a task

To execute a task from your application, your application needs to connect to the core through [gRPC](https://grpc.io/) and using the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go). 

{% tabs %}
{% tab title="Request" %}
| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| **service** | [`Service`](../service/service-file.md) | Required | Object containing the service definition loaded from the yml [service file](../service/service-file.md). |
| **taskKey** | `String` | Required | The task's key defined in the [service file](../service/service-file.md). |
| **taskData** | `String` | Required | The task's inputs in JSON format. |

```javascript
{
  "service": {
    ...
    "tasks": {
      "taskX": {
        "inputs": {
          "inputX": { "type": "String" }
        },
        "outputs": {
          "outputX": {
            "data": {
              "outputValX": { "type": "String" }
            }
          }
        }
      }
    },
    ...
  },
  "taskKey": "taskX",
  "taskData": "{\"inputX\":\"input value\"}"
}
```
{% endtab %}

{% tab title="Reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- |
| **error** | `String` | A string that contains the error if any |
| **executionID** | `String` | The ID of the execution |

```javascript
{
  "error": "",
  "executionID": "xxxxx"
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

