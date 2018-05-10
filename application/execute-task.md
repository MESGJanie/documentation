# Execute a task

## Why execute a service's task?

Applications can execute a service's task allowing you to reuse the maximum number of already-built logic, and enjoy the MESG ecosystem.

## Execute a service's task

To execute a task, applications need to connect to Core through [gRPC](https://grpc.io/) and use the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go). Core will reply with an `executionID`that identifies the task's execution. To get the output of the task's execution, the application has to listen for an [execution output.](execute-task.md#listen-for-execution-outputs)

{% tabs %}
{% tab title="Request" %}
### `Client.ExecuteTask`

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| **service** | [`Service`](../service/service-file.md) | Required | An object containing the service definition loaded from the yml [service file](../service/service-file.md). |
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
| **error** | `String` | A string that contains the error, if any. |
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
const api = grpc.load(__dirname + '/api/client/api.proto').api
const core = new api.Core(
  process.env.MESG_ENDPOINT,
  grpc.credentials.createInsecure()
)
​
core.ExecuteTask({
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
```go
package main

import (
	"context"
	"fmt"

	"github.com/mesg-foundation/core/api/core"
	"github.com/mesg-foundation/core/service"
	"google.golang.org/grpc"
)

func main() {
	connection, _ := grpc.Dial(":50052", grpc.WithInsecure())
	cli := core.NewCoreClient(connection)
	res, _ := cli.ExecuteTask(context.Background(), &core.ExecuteTaskRequest{
		Service:  &service.Service{},
		TaskKey:  "xxxx",
		TaskData: "{\"foo\":\"bar\"}",
	})
	fmt.Println(res.ExecutionID)
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



## Listen for task execution outputs

The execution of tasks can take a long time to finish depending on the action they are completing, so outputs are sent back asynchronously. To listen for task execution's outputs, applications need to open a stream with the Core through [gRPC](https://grpc.io/) and use the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go).

{% hint style="info" %}
Outputs are sent asynchronously. Make sure that the application listens for outputs before it executes a task, otherwise it will miss the outputs.
{% endhint %}

{% tabs %}
{% tab title="Request" %}
### `Client.ListenResult`

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
const api = grpc.load(__dirname + '/api/client/api.proto').api
const core = new api.Core(
  process.env.MESG_ENDPOINT,
  grpc.credentials.createInsecure()
)

const listenResultStream = core.ListenResult({
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
package main

import (
	"context"
	"fmt"

	"github.com/mesg-foundation/core/api/core"
	"github.com/mesg-foundation/core/service"
	"google.golang.org/grpc"
)

func main() {
	connection, _ := grpc.Dial(":50052", grpc.WithInsecure())
	cli := core.NewCoreClient(connection)
	stream, _ := cli.ListenResult(context.Background(), &core.ListenResultRequest{
		Service: &service.Service{},
	})
	for {
		result, _ := stream.Recv()
		fmt.Println(result.ExecutionID, result.OutputKey, result.OutputData)
		// TODO process result
	}
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



