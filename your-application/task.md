# Task

The task is the action executed each time an event is triggered. Every task will have a different purpose and parameters that you'll need to fulfill.

If you want to make a complex workflow, you may need to trigger multiple tasks. For every workflow, you'll be able to set a single task, or a list of tasks that will be executed in the order you've defined.

#### Example

Let's use an example of one task `taskX` with the parameters `key` and `value` and suppose that you have one event `eventX` that returns the data `eventValue` and you want to `process` this data before sending it to `taskX`. You could have a  workflow similar to this :

```javascript
└── eventX() // event that you need to connect
  ├── process(value: event.eventValue) // process the data from the event with the value from the event
  └── taskX(key: 'keyX', value: task[0].result) // execute the task with the key 'keyX' and the value from the result of the task[0]
```

## Properties

### service

> String required

Service is the type of task to connect, this will be the ID of the service you want to connect. This service will run on nodes in the network and will have different kinds of tasks. You can find the list of services running on the MESG platform directly in  or on .

### task

> String required

The task from the service that you want execute. This task will be executed by a selected node on the network and will need to receive parameters that you can find in the description of the task or in the service detail that you can find in  or on .

### parameters

> Object

Parameters need to be set for the task according to the parameters required from task documentation. If your task needs the parameters `foo` and `bar` you will need to send value for those two parameters like `foo=3` and `bar=5`.

In many cases, you may want to bind the data from the event to the parameter of the task, in this case you will be able to use the data from the event. In the case of having a workflow with multiple tasks, you will also be able to connect every result from one of the previous tasks as a parameter of the new one.

### filters

> Object

List of filter to apply before the execution of the task. Only if all filters are valid will a task be executed. This let you customize your workflow and add some processing conditions. All filters will be obfuscated based on the hashing of the value.

### whitelist

> String\[\] optional

The whitelist parameter permits only specific nodes to execute a task. The value for the whitelist will be on a list of public node keys. If this parameter is not set, any node in the network will have the possibility of running this service. In order to accept any node to execute this task you can also pass the `*` value.

### blacklist

> String\[\] optional

The blacklist parameter allows you to reject specific nodes on the network. The value for the blacklist will be on a list of public node keys. If this parameter is not set, no nodes will be blacklisted and any node on the network will have the possibility of executing the task.

