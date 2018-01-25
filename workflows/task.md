# Task

The task will be the action executed every time the selected event is triggerd. Every task will have different purpose and various parameters that you will need to fullfil.

When you want to make complex workflow you might need to trigger multiple tasks. For every workflow you will be able to set a single task or a list of tasks that will be executed in the order you defined.

###### Example

Let's take the example of one task `taskX` with the parameters `key` and `value` and suppose that you have one event `eventX` that return the data `eventValue` and you want to `process` this data before send it to `taskX`. You might have a similar workflow :

```javascript
└── eventX() // event that you need to connect
  ├── process(value: event.eventValue) // process the data from the event with the value from the event
  └── taskX(key: 'keyX', value: task[0].result) // execute the task with the key 'keyX' and the value from the result of the task[0]
```

## Properties

#### service

> String required

The type of the task to connect, this will be the id of the service to connect. This service will ran on some nodes in the network and will have different kind of tasks. You can find the list of services running on the MESG platform directly in {{ book.wallet }} or on {{ book.endpoints.services }}.

#### task

> String required

The task from the service that you want execute. This task will be executed by a selected node of the network and will need to receive some parameters that you can find on the description of the task in the service detail that you can find in {{ book.wallet }} or on {{ book.endpoints.services }}.

#### parameters

> Object

Parameters needs to be set for the task accordingly to the parameters required from the task documentation. If you task needs the parameters `foo` and `bar` you will need to send value for thoses two parameters like `foo=3` and `bar=5`.

In many case you may want to bind the data from the event to the parameter of the task, in this case you will be able to use the data from the event. In case you have a workflow with multiple tasks you will also be able to connect every result from one of the previous task as parameter of the new one.

#### whitelist

> String[] optional

The whitelist parameter permits to accept some specific nodes only to execute this task. The value for the whitelist will be a list of node public keys. If this parameter is not set, any nodes of the network will have the possibility to run this service. In order to accept any node to execute this task you can also pass the `*` value. 

#### blacklist

> String[] optional

The blacklist parameter permits to reject some specific nodes from the network. The value for the blacklist will be a list of node public keys. If this parameter is not set, no nodes will be blacklisted and any node on the network will have the possibility to execute the task.
