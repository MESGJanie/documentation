# Create Your Application

Applications and solutions are built on MESG by attaching an event on one service to a task on another service. These can be configured in any order, and you can create chain reactions or timed events.

## Steps in an Application

A basic application is built of two main steps, listening for events, and the execution of tasks.

1. **Event**
2. **Task**

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

