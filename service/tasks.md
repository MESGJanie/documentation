# Listen for Tasks

## Why listen for tasks?

The Service needs to receive the command sent by the Core in order to execute any desired task. Every time a command is received, it will ensure that the sender is the Core, then it will check if it can handle the command, and if so, it will execute it. Once executed, it will reply to the Core with the result of the command.

### Steps to create a task

In order to define your task you need to :

* [ ] [Add your definition](tasks.md#create-your-task) in your [`mesg.yml`](service-file.md) file
* [ ] [Listen for task execution](tasks.md) from the [Core](../start-here/core.md)
* [ ] [Submit the outputs](tasks.md#submit-outputs-of-your-execution) of your task

## Create your task

{% tabs %}
{% tab title="Detail" %}
The first step is to declare the tasks that you service will be able to execute in your [`mesg.yml`](service-file.md) file. The events should be indexed by their id and should describe the following attributes :

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- | --- |
| **name** | `id` | `String` | Name of your event, if not set, the name will be the id selected for the task |
| **description** | `""` | `String` | Description of your task, what the task is doing and why it is useful |
| **inputs** | `{}` | `map<id,`[`Input`](tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of inputs that your task needs in order to be executed |
| **outputs** | `{}` | `map<id,`[`Outputs`](tasks.md#outputs-data)`>` | Map of outputs that your task will emit. Your task can declare multiple outputs but can only submit one output per execution. |
| **secrets** | `{}` | `map<id,`[`Secret`](tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of secrets that your task may need. Secrets are environmental variables that are set directly by the node. |

### Outputs data

| Attribute | Default value | Type | Description |
| --- | --- | --- | --- |
| name | `id` | `String` | Name of your output, default is the id you defined |
| description | `""` | `String` | A description of your output, what kind of output, what and how is it useful |
| data | `{}` | `map<id,`[`Output`](tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of data that your output will return |

### Data of your parameter \(Input/Output/Secret\)

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **name** | `id` | `String` | Name or your parameter, default: the id that you defined |
| **description** | `""` | `String` | Description of your parameter |
| **type** | `String` | [`Type`](tasks.md#type-of-your-data) | Type of your parameter |
| **optional** | `false` | `Boolean` | If true, this parameter is considered as optional and might be empty  |

### Type of your data {#type-of-your-data}

You can send different types of data. This type can be one of the following :

* `String`
* `Boolean`
* `Number`
* `Object`
{% endtab %}

{% tab title="Example" %}
Here is an example of what your event might look like in your [`mesg.yml`](https://docs.mesg.tech/service/service-file) file :

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

Your service needs to listen tasks sent by the [core](../start-here/core.md). In order to do that you need to use the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go) and [gRPC](https://grpc.io/) to listen for the execution. When you start listening, a stream will be open between your service and the core and you will receive new tasks from the core.

{% hint style="info" %}
Consider listening for task when your service is ready. If your service needs to synchronise some data first, you should wait for this synchronisation before listening for tasks.
{% endhint %}

### Task.Listen

{% tabs %}
{% tab title="Request" %}
| **Attribute** | **Type** | **Required** | **Description** |
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
| **taskID** | `String` | A unique ID for the task that allows to track the result in an asynchronous way |
| **error** | `String` | A string that contains the error if an error is present |
| **task** | `String` | Name of the task to execute |
| **data** | `String` | Inputs of the task serialized in JSON |

```javascript
{
    "taskID": "xxxxxx",
    "error": "",
    "task": "taskX",
    "data": "{\"inputX\":\"Hello world!\",\"inputY\":true}"
}
```
{% endtab %}
{% endtabs %}

#### Exemple

{% tabs %}
{% tab title="Node" %}
// TODO: add exemple in node
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

	"github.com/mesg-foundation/core/types"
	"google.golang.org/grpc"
	yaml "gopkg.in/yaml.v2"
)

func main() {
	content, _ := ioutil.ReadFile("./mesg.yml")
	var service types.ProtoService
	yaml.UnmarshalStrict(content, service)

	connection, _ := grpc.Dial(os.Getenv("MESG_ENDPOINT"), grpc.WithInsecure())
	cli := types.NewTaskClient(connection)

	stream, _ := cli.Listen(context.Background(), &types.ListenTaskRequest{
		Service: &service,
	})

	for {
		res, _ := stream.Recv()
		fmt.Println("receive task", res.Task, "with inputs", res.Data)
	}
}


```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

## Submit outputs of your execution

Once your task finish its processing you will need to send the outputs of the execution back to the [core](../start-here/core.md). You will still need to use the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go) and [gRPC](https://grpc.io/) to submit your results. Your task can only submit a single type of output per execution. Even if your task is declaring multiple kind of outputs only one should be submitted at the time. 

{% tabs %}
{% tab title="Request" %}
| **Attribute** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| **taskID** | `String` | required | The `taskID` received from the [listen](tasks.md#listen-for-task-executions) stream. |
| **output** | `String` | required | The id of the output as defined in the [declaration of your output](tasks.md#create-your-task). |
| **data** | `String` | required | The data for the output you want to send encoded in JSON. The data should match the one you defined in the [declaration of your output](tasks.md#create-your-task). |

```javascript
{
    "taskID": "xxxxxx",
    "output": "outputX"
    "data": "{\"outputDataX\":\"super result\",\"outputDataY\":42}"
}
```
{% endtab %}

{% tab title="Reply" %}
| **Attribute** | **Type** | **Description** |
| --- | --- |
| **error** | `String` | Error when submitting the output of the task if an error happened. |

```javascript
{
    "error": ""
}
```
{% endtab %}
{% endtabs %}

#### Exemple

{% tabs %}
{% tab title="Node" %}
// TODO: add exemple in node
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
cli := types.NewResultClient(connection)
...
// do my processing
outputX := OutputX{
  OutputDataX: "...",
  OutputDataY: 0,
}
...
outputXResult, _ := json.Marshal(outputX)

res, _ := cli.Submit(context.Background(), &types.SubmitResultRequest{
    Data: outputXResult,
    Output: "outputX",
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



