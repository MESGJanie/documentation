# Create Your Application

Applications or business solutions are built on MESG by attaching an event on one service to a task on another service. These can be configured in any order, and you can create chain reactions or timed events.

## Steps in an Application

A basic application is built of two main steps, listening for events, and execution of tasks.

1. [**Event**](listen.md)
2. [**Task**](execute-task.md)

## Event

Event is the external action that triggers the flow of tasks. It directs your application to listen for an event. We use the word "when" to describe how events function in our [use cases](use-cases.md).

```text
  event.listen
```

## Task

Task is action that the applicaiton will execute after it is triggered. We use the word "then" to describe how Task functions in our [use cases](use-cases.md).

```text
  task.execute
```

