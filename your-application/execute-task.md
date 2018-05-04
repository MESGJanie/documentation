# Execute a task

## Why execute a task?

Your Application executes tasks to feed information in real time to the MESG ecosystem, or to trigger a task on any other technology that is connected to your Application. 

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

## Listen for execution outputs

When you execute a task you might want to listen for it's outputs too. Because tasks can take a long time depending on the action they are doing, outputs are send asynchronously and you need to listen for outputs as a separate process. In order to listen for events you need to connect to the core through [gRPC](https://grpc.io/) and using the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go). 

{% hint style="info" %}
Outputs are send asynchronously. Make sure that you listen for outputs before you try to execute a task otherwise you might miss the output of this task execution.
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
| **Name** | **Type** | **Description** |
| --- | --- | --- | --- | --- | --- |
| **executionID** | `String` | The ID of the execution of the task that created this output. |
| **error** | `String` | A string that contains the error if an error is present |
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

