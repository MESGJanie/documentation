# Execute task

## Why execute a service's task?

Applications can execute tasks of service in order to reuse a maximum of already build logic and enjoy the MESG ecosystem.

## Execute a service's task

To execute a task, applications need to connect to the core through [gRPC](https://grpc.io/) and use the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go). The core will reply with an `executionID`that identify the task execution. To get the output of the task execution, the application has to listen for [execution output.](execute-task.md#listen-for-execution-outputs)

{% tabs %}
{% tab title="Request" %}
### Client.ExecuteTask

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
| **error** | `String` | A string that contains the error if any. |
| **executionID** | `String` | The ID of the execution. |

```javascript
{
  "error": "",
  "executionID": "xxxxx"
}
```
{% endtab %}
{% endtabs %}

### Examples

{% tabs %}
{% tab title="Node" %}
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const grpc = require('grpc')
const yaml = require('js-yaml')
const fs = require('fs')
const api = grpc.load(__dirname + '/api/client/api.proto').client
const client = new api.Client(
  process.env.MESG_ENDPOINT,
  grpc.credentials.createInsecure()
)
​
client.ExecuteTask({
  service: yaml.safeLoad(fs.readFileSync("./mesg.yml")),
  taskKey: "eventX",
  taskData: JSON.stringify({
    foo: "hello",
    bar: false
  })
}, (err, reply) => {
  // handle response if needed
})
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



## Listen for task execution outputs

Task execution can take a long time to proceed depending on the action they are completing, so outputs are sent back asynchronously. To listen for task executions outputs, applications need to open a stream with the Core through [gRPC](https://grpc.io/) and use the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go).

{% hint style="info" %}
Outputs are send asynchronously. Make sure that the application listens for outputs before it executes a task, otherwise it will miss the outputs.
{% endhint %}

{% tabs %}
{% tab title="Request" %}
### Client.ListenResult

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
| **executionID** | `String` | The execution ID of this output. |
| **error** | `String` | A string that contains the error if an error is present. |
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

### Examples

{% tabs %}
{% tab title="Node" %}
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const grpc = require('grpc')
const yaml = require('js-yaml')
const fs = require('fs')
const api = grpc.load(__dirname + '/api/client/api.proto').client
const client = new api.Client(
  process.env.MESG_ENDPOINT,
  grpc.credentials.createInsecure()
)

const listenResultStream = client.ListenResult({
  service: yaml.safeLoad(fs.readFileSync("./mesg.yml")),
})
listenResultStream.on('error', function(error) {
  // An error has occurred and the stream has been closed.
})
listenResultStream.on('data', function(data) {
  console.log('receive', data)
})
listenResultStream.on('status', function(status) {
  // process status
})

// Now it's safe to execute a task
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="Go" %}
{% code-tabs %}
{% code-tabs-item title="main.go" %}
```go
// TODO
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



