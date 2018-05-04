# Create Your Application

Applications or business solutions are built on MESG by attaching an event on one service to a task on another service. These can be configured in any order, and you can create chain reactions or timed events.

If you want your application to easily migrate to the future release with no code needed to create your application you should start thinking your application with Event Driven Architecture.

In order to create a maintainable and evolutive application you should try only to react to event. Every task that your application have to do should react to an event. Example: When my user signup then send an email.

By creating your application that way, you embrace the philosophy of MESG and create an application become really easy.

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

