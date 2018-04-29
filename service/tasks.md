# Listen for Tasks

## Why listen for tasks?

The Service needs to receive the command sent by the Core in order to execute any desired task. Every time a command is received, it will ensure that the sender is the Core, then it will check if it can handle the command, and if so, it will execute it. Once executed, it will reply to the Core with the result of the command.

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
| **secrets** | `{}` | `map<id,`[`Secret`](tasks.md#data-of-your-parameter-input-output-secret)`>` | Map of secrets that your task may needs. Secrets are environmental variables that are set directly by the node. |

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

{% tab title="Exemple" %}
Here is an exemple of what your event might looks like in your [`mesg.yml`](https://docs.mesg.tech/service/service-file) file :

{% code-tabs %}
{% code-tabs-item title="mesg.yml" %}
```yaml
...
tasks:
    taskX:
        name: "Task X"
        description: "This is the task X"
        inputs:
            foo:
                name: "Foo"
                description: "Foo is the first data"
                type: String
                optional: false
            bar:
                name: "Bar"
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



## Submit results of your execution

