# Workflow File

In order to define your workflow, you can use a `YAML` formatted file that details all of your needs.

This file needs to contain a [source event](source.md) and one or multiple [tasks](task.md).

You can find the raw file example [here](https://github.com/mesg-foundation/documentation/blob/master/workflow/example.yml).

## Notes on parameters

### Parameter type

Parameters need to be sent with the right type \(Number, String, Object, Array, Boolean\) according to the service's documentation.

### Parameter binding

Every parameter can be bound to a dynamic value, workflow execution or workflow deployment. Here are the syntaxes to achieve this:

#### Workflow variables

Your workflow will need to access the variables from the event and other steps from the workflow. In order to achieve this, you can refer to the result directly on your template. The value will be applied during the processing of the workflow.

With workflow variables, you can have access to the **inputs** of the workflow, the **source** event data or any **task** result.

```yaml
source:
  service: Ethereum
  ...
...
tasks:
  processAmount:
    ...
    parameters:
      amount: ${workflow:source.amount}
  notify:
    ...
    parameters:
      title: ${workflow:inputs.notificationTitle}
      value: ${workflow:processAmount.result}
inputs:
  notificationTitle: "Title for the notification"
```

#### Environment variables

In many cases, you won't need to write private data on the file. In case of wanting to use a versioning system, if you want to have different environments etc., you can use environmental variables. The value will be replaced when the workflow is deployed.

```yaml
...
tasks:
  xxx:
    ...
    parameters:
      secret: ${env:SECRET}
```

#### Default value

You may need to have some default values in case the variable is not present during deployment or processing. In order to do this, you can add another argument to the template.

```yaml
parameterX: ${env:PARAMETER_X, 42}
```

## Task processing

If your workflow contains multiple tasks to execute, the will automatically optimize the workflow to parallelize the tasks. In order to parallelize the execution, a dependency graph will be generated with the source event as a root node and to parallelize all children with the same depth.

### Instant tasks

If a task doesn't have a direct or indirect dependency on the event, this task will be executed from your computer while deploying, and will not be executed every time the workflow starts, but will use the value from the result during deployment.

### Concurrency

Because of the parallelization of the task, it is no guarantee to have a task executed before another one if they don't have any dependencies. If you want to ensure that a task runs after another one you need to create a [workflow variable](workflow-file.md#workflow-variables) from this task.

### Cycles

Because of the dependency generation, your workflow cannot contain two tasks that depend on each other \(even if they are of different depths\). If you have a cycle in your workflow, you will have an error while validating or deploying.

