# Create Your Application

Applications and business solutions are built on MESG by attaching an event on one service to a task on another service. These can be configured in any order, and you can create chain reactions or timed events.

Future versions of MESG will not require users to code. Instead, you'll send a configuration file, which is like an order slip, listing all of the events and corresponding tasks you'd like the MESG Network to execute for you.   
  
As long as the [Services](../service/what-is-a-service.md) \(technologies\) you want to use in your solution have been connected to the MESG Infrastructure already, you will be able to list them in the configuration file. If they aren't been connected yet, you can connect Services yourself, with some coding.

The concept of events and corresponding tasks is called Event Driven Architecture. This is how the configuration file in future releases of MESG is laid out, so if you want your application to be compatible with future releases of MESG, we recommend to start building your application based on Event Driven Architecture while we finish completing the Infrastructure. 

In order to create a an application that's maintainable and compatible with future releases, build your applications to react to events. Events in your application initiate a connection. \(e.g. receiving an email, a new deposit, a full battery, the first of the month, a delayed flight, etc.\)

Tasks in your application are reactions to events. \(send an email, notify me on my watch, put the car into standby, issue refunds, transfer funds, open a new account, turn on the lights, etc.\)

By creating an application based in Event Oriented Architecture, you embrace the philosophy of MESG and make an application that becomes really easy.

### Source of events

{% hint style="info" %}
The event is the **when** for your application
{% endhint %}

The source of an event can come from two different parts of your service :

* [Events from services](listen.md)
* [Outputs from the tasks of the services](execute-task.md)

### Task to execute

{% hint style="info" %}
The task is the **then **for your application
{% endhint %}

When one event is coming then the only thing to do is to [execute a task](execute-task.md) of the service that you want.

You can find some example in the [use cases](use-cases.md) page.

