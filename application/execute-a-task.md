# Execute a task

## Why execute a service's task?

Applications can execute a service's task allowing you to reuse the maximum number of already-built logic and enjoy the MESG ecosystem.

## Execute a service's task

To execute a task, applications need to connect to Core through [gRPC](https://grpc.io/) and use the [Protobuffer definition](https://github.com/mesg-foundation/core/blob/dev/api/core/api.proto). Core will reply with an `executionID`that identifies the task's execution. To get the output of the task's execution, the application has to listen for an [execution output.](execute-a-task.md#listen-for-execution-outputs)

{% tabs %}
{% tab title="Request" %}
### `Core.ExecuteTask`

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| **serviceId** | `String` | Required | ID of the service. |
| **taskKey** | `String` | Required | The task's key defined in the [service file](../service/service-file.md). |
| **taskData** | `String` | Required | The task's inputs in JSON format. |

```javascript
{
  "serviceID": "v1_fe25be776e1e256400c77067a1cb7666",
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
const api = grpc.load(__dirname + '/api/client/api.proto').api
const core = new api.Core(
  "localhost:50052",
  grpc.credentials.createInsecure()
)
​
core.ExecuteTask({
  serviceID: "v1_fe25be776e1e256400c77067a1cb7666",
  taskKey: "taskX",
  taskData: JSON.stringify({
    inputX: "input value"
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
		ServiceID:  "v1_fe25be776e1e256400c77067a1cb7666",
		TaskKey:  "taskX",
		TaskData: "{\"inputX\":\"input value\"}",
	})
	fmt.Println(res.ExecutionID)
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



