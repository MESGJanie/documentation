# Listen for Tasks

## Why listen for tasks?

The Service needs to receive the command sent by the Core in order to execute any desired task. Every time a command is received, it will ensure that the sender is the Core, then it will check if it can handle the command, and if so, it will execute it. Once executed, it will reply to the Core with the result of the command.

## Steps to follow

To implement tasks in your Service, you need to :

* [ ] [Add the task definition](tasks.md#create-your-task) in the Service's [`mesg.yml`](service-file.md) file
* [ ] [Listen for task execution](tasks.md) from the [Core](../start-here/core.md)
* [ ] [Submit the outputs](tasks.md#submit-outputs-of-your-execution) of the task

## Create a Task

{% tabs %}
{% tab title="Detail" %}
The first step is to declare the tasks that the service will be able to execute in the service's [`mesg.yml`](service-file.md) file. The events should be indexed by their id and should describe the following attributes :

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- | --- |
| **name** | `id` | `String` | Name of the task, if not set, the name will be the id of the task. |
| **description** | `""` | `String` | Description of the task, what the task is doing and why it is useful. |
| **inputs** | `{}` | `map<id,`[`Input`](tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of inputs that the task needs in order to be executed. |
| **outputs** | `{}` | `map<id,`[`Outputs`](tasks.md#outputs-data)`>` | Map of outputs that the task will emit. The task can declare multiple outputs but can only submit one output per execution. |
| **secrets** | `{}` | `map<id,`[`Secret`](tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of secrets that the task may need. Secrets are environmental variables that are set directly by the user. |

### Outputs

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- |
| **name** | `id` | `String` | Name of the output, default is the id. |
| **description** | `""` | `String` | A description of the output, what kind of output, what and how is it useful. |
| **data** | `{}` | `map<id,`[`Output`](tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of data that the output will return. |

### Input, Output and Secret parameter

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **name** | `id` | `String` | Name or the parameter, default: the id. |
| **description** | `""` | `String` | Description of the parameter. |
| **type** | `String` | [`Type`](tasks.md#type-of-your-data) | Type of the parameter. |
| **optional** | `false` | `Boolean` | If true, this parameter is considered as optional and might be empty. |

### Type of parameter {#type-of-your-data}

The parameter can be one of the following:

* `String`
* `Boolean`
* `Number`
* `Object`
{% endtab %}

{% tab title="Example" %}
Example of a task definition in a [`mesg.yml`](https://docs.mesg.tech/service/service-file) file :

{% code-tabs %}
{% code-tabs-item title="mesg.yml" %}
```yaml
...
tasks:
    taskX:
        name: "Task X"
        description: "This is the task X"
        inputs:
            inputX:
                name: "Input x"
                description: "Foo is the first data"
                type: String
                optional: false
            inputY:
                name: "Input y"
                description: "Bar is the second data"
                type: Boolean
                optional: true
        outputs:
            outputX:
                name: "OutputX"
                description: "Output X"
                data:
                    outputDataX:
                        name: "Output data x"
                        description: "Description about output data x"
                        type: String
                    outputDataY:
                        name: "Output data y"
                        description: "Description about output data y"
                        type: Number
            outputY:
                ...
        secrets:
            SECRET_X:
                name: "SecretX"
                type: String
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

## Listen for task executions

To listen for task to execute, the service needs to open a stream with the core using the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go) and [gRPC](https://grpc.io/). Every task received on the stream need to be executed by the service and the output [submitted](tasks.md#submit-outputs-of-your-execution) back to the core.

{% hint style="info" %}
Consider listening for task when your service is ready. If your service needs to synchronize some data first, you should wait for this synchronization before listening for tasks.
{% endhint %}

{% tabs %}
{% tab title="Request" %}
### Service.ListenTask

| **Name** | **Type** | **Required** | **Description** |
| --- | --- |
| **service** | [`Service`](service-file.md) | Required | Object containing the service definition loaded from the yml service file. |

```javascript
{
    "service": {
      ...
      "tasks": {
        "taskX": {
          "inputs": {
            "foo": { "type": "String" },
            "bar": { "type": "Boolean" }
          }
          ...
        }
      },
      ...
    }
}
```
{% endtab %}

{% tab title="Stream reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **executionID** | `String` | A unique ID for the task that allows to track the result in an asynchronous way |
| **error** | `String` | A string that contains the error if an error is present |
| **taskKey** | `String` | Key of the task to execute \(as in your `mesg.yml` file\) |
| **inputData** | `String` | Inputs of the task serialized in JSON |

```javascript
{
    "executionID": "xxxxxx",
    "error": "",
    "taskKey": "taskX",
    "inputData": "{\"inputX\":\"Hello world!\",\"inputY\":true}"
}
```
{% endtab %}
{% endtabs %}

### Examples

{% tabs %}
{% tab title="Node" %}
// TODO: add example in node
{% endtab %}

{% tab title="Go" %}
{% code-tabs %}
{% code-tabs-item title="main.go" %}
```go
package main

import (
	"context"
	"fmt"
	"io/ioutil"
	"os"

	api "github.com/mesg-foundation/core/api/service"
	"github.com/mesg-foundation/core/service"
	"google.golang.org/grpc"
	yaml "gopkg.in/yaml.v2"
)

func main() {
	content, _ := ioutil.ReadFile("./mesg.yml")
	var service service.Service
	yaml.UnmarshalStrict(content, &service)

	connection, _ := grpc.Dial(os.Getenv("MESG_ENDPOINT"), grpc.WithInsecure())
	cli := api.NewServiceClient(connection)

	stream, _ := cli.ListenTask(context.Background(), &api.ListenTaskRequest{
		Service: &service,
	})

	for {
		res, _ := stream.Recv()
		fmt.Println("receive task", res.TaskKey, "with inputs", res.InputData)
	}
}


```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

## Submit outputs of task executions

Once the task execution is finished, the service have to send the outputs of the execution back to the [core](../start-here/core.md) using the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go) and [gRPC](https://grpc.io/). Only one output can be submitted per execution even if the task declared multiple outputs.

{% tabs %}
{% tab title="Request" %}
### Service.SubmitResult

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| **executionID** | `String` | required | The `executionID` received from the [listen](tasks.md#listen-for-task-executions) stream. |
| **outputKey** | `String` | required | The id of the output as defined in the [output's declaration](tasks.md#create-your-task). |
| **outputData** | `String` | required | The output's data encoded in JSON. The data should match the one defined in the [output's declaration](tasks.md#create-your-task). |

```javascript
{
    "executionID": "xxxxxx",
    "outputKey": "outputX"
    "outputData": "{\"outputDataX\":\"super result\",\"outputDataY\":42}"
}
```
{% endtab %}

{% tab title="Reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- |
| **error** | `String` | Error during submission of the execution's output. |
| **executionID** | `String` | The execution's id. |

```javascript
{
    "error": ""
    "executionID": "xxxxxx"
}
```
{% endtab %}
{% endtabs %}

### Examples

{% tabs %}
{% tab title="Node" %}
// TODO: add example in node
{% endtab %}

{% tab title="Go" %}
{% code-tabs %}
{% code-tabs-item title="main.go" %}
```go
type OutputX struct {
	OutputDataX string `json:"outputDataX"`
	OutputDataY int    `json:"outputDataY"`
}
// Job to listen event, see "Listen for task executions" part
cli := types.NewServiceClient(connection)
...
// do my processing
outputX := OutputX{
  OutputDataX: "...",
  OutputDataY: 0,
}
...
outputXResult, _ := json.Marshal(outputX)

res, _ := cli.Submit(context.Background(), &types.SubmitResultRequest{
    ExecutionId: "...",
    OutputKey: "outputX",
    OutputData: outputXResult,
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



