# Create Your Application

Applications or business solutions are built on MESG by attaching an event on one service to a task on another service. These can be configured in any order, and you can create chain reactions or timed events.

Future versions of MESG will not require users to code. Instead, you'll send a simple text configuration file, which is like an order slip, listing all of the events and corresponding tasks you'd like the MESG Network to execute for you.   
  
As long as the [Services](../service/what-is-a-service.md) \(technologies\) you want to use in your solution have been connected to the MESG Infrastructure already, you can list it in the configuration file. If it hasn't been connected yet, you can connect it yourself, with some coding.

The concept of events and corresponding tasks is called Event Driven Architecture. This is how the configuration file \(order slip with no code needed\) in future releases of MESG will function, so if you want your application to be compatible with future releases of MESG, start building your application based on Event Driven Architecture while we finish completing the MESG Infrastructure. 

In order to create a an application that's maintainable and compatible with future releases of MESG, build applications that react to events. \(e.g. receiving an email, a new deposit, a full battery, the first of the month, a delayed flight etc.\)

Tasks in your application are reactions to events. \(send an email, notify me on my watch, put the car into standby, issue refunds, transfer funds, open a new account, turn on the lights etc.\)

By creating your application in this way, you embrace the philosophy of MESG and make an application becomes really easy.

### Source of events

{% hint style="info" %}
The event is the **when** for your application
{% endhint %}

Your source of event can come from two different part of your services :

* [Events from services](listen.md)
* [Outputs from the tasks of the services](execute-task.md)

### Task to execute

{% hint style="info" %}
The task is the **then **for your application
{% endhint %}

When one event is coming then the only thing to do is to [execute a task](execute-task.md) of the service that you wants.

You can find some example in the [use cases](use-cases.md) page.

