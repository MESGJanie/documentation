# Listen for Tasks

## Why listen for tasks?

The Service needs to receive a command sent by Core in order to execute any desired task. Every time a command is received, it will ensure that the sender is Core, then it will check if it can handle the command, and if so, it will execute it. Once executed, it will reply to Core with the result of the command.

## Steps to follow

To implement tasks in your Service, you need to :

* [ ] [Add the task definition](listen-for-tasks.md#create-your-task) in the Service's [`mesg.yml`](service-file.md) file
* [ ] [Listen for task execution](listen-for-tasks.md) from the [Core](../start-here/core.md)
* [ ] [Submit the outputs](listen-for-tasks.md#submit-outputs-of-your-execution) of the task

## Create a Task

{% tabs %}
{% tab title="Detail" %}
The first step is to declare the tasks that the service will be able to execute in the service's [`mesg.yml`](service-file.md) file. The events should be indexed by their ID and should describe the following attributes :

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- | --- |
| **name** | `id` | `String` | Name of the task, if not set, the name will be the ID of the task. |
| **description** | `""` | `String` | Description of the task, what the task is doing and why it is useful. |
| **inputs** | `{}` | `map<id,`[`Input`](listen-for-tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of inputs that the task needs in order to be executed. |
| **outputs** | `{}` | `map<id,`[`Outputs`](listen-for-tasks.md#outputs-data)`>` | Map of outputs that the task will emit. The task can declare multiple outputs but can only submit one output per execution. |
| **secrets** | `{}` | `map<id,`[`Secret`](listen-for-tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of secrets that the task may need. Secrets are environmental variables that are set directly by the user. |

### Outputs

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- |
| **name** | `id` | `String` | Name of the output, the default is the ID. |
| **description** | `""` | `String` | A description of the output, what kind of output, and how is it useful. |
| **data** | `{}` | `map<id,`[`Output`](listen-for-tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of the data  the output will return. |

### Input, Output and Secret parameter

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **name** | `id` | `String` | Name or the parameter, default: the ID. |
| **description** | `""` | `String` | Description of the parameter. |
| **type** | `String` | [`Type`](listen-for-tasks.md#type-of-your-data) | Type of the parameter. |
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

To listen for task to execute, the Service needs to open a stream with Core using the [Protobuffer definition](https://github.com/mesg-foundation/core/blob/dev/api/service/api.proto) and [gRPC](https://grpc.io/). Every task received on the stream needs to be executed by the Service and the output [submitted](listen-for-tasks.md#submit-outputs-of-your-execution) back to Core.

{% hint style="info" %}
Consider listening for tasks when your service is ready. If your service needs to synchronize some data first, you should wait for this synchronization before listening for tasks.
{% endhint %}

{% tabs %}
{% tab title="Request" %}
### `Service.ListenTask`

| **Name** | **Type** | **Required** | **Description** |
| --- | --- |
| **token** | `String` | Required | The token given by the Core as environment variable `MESG_TOKEN` |

```javascript
{
    "token": "TOKEN_FROM_ENV"
}
```
{% endtab %}

{% tab title="Stream reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **executionID** | `String` | A unique ID for the task that allows you to track the result in an asynchronous way |
| **error** | `String` | A string that contains the error, if an error is present |
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
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const MESG = require('mesg-js').service()

MESG.listenTask({
// task      inputs           outputs
  taskX: ({ foo, bar }, { outputX, outputY }) => outputX({ foo })
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
	"io/ioutil"
	"os"

	api "github.com/mesg-foundation/core/api/service"
	"google.golang.org/grpc"
	yaml "gopkg.in/yaml.v2"
)

func main() {
	connection, _ := grpc.Dial(os.Getenv("MESG_ENDPOINT"), grpc.WithInsecure())
	cli := api.NewServiceClient(connection)

	stream, _ := cli.ListenTask(context.Background(), &api.ListenTaskRequest{
		Token: os.Getenv("MESG_TOKEN"),
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

Once the task execution is finished, the Service has to send the outputs of the execution back to [Core](../start-here/core.md) using the [Protobuffer definition](https://github.com/mesg-foundation/core/blob/dev/api/service/api.proto) and [gRPC](https://grpc.io/). Only one output can be submitted per execution even if the task has declared multiple outputs.

{% tabs %}
{% tab title="Request" %}
### `Service.SubmitResult`

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| **executionID** | `String` | required | The `executionID` received from the [listen](listen-for-tasks.md#listen-for-task-executions) stream. |
| **outputKey** | `String` | required | The ID of the output as defined in the [output's declaration](listen-for-tasks.md#create-your-task). |
| **outputData** | `String` | required | The output's data encoded in JSON. The data should match the one defined in the [output's declaration](listen-for-tasks.md#create-your-task). |

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
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const MESG = require('mesg-js').service()

MESG.listenTask({
// task      inputs           outputs
  taskX: ({ foo, bar }, { outputX, outputY }) => outputX({ foo })
})

```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

