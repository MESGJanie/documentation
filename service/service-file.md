# Service file

## Service file

To define a [Service](what-is-a-service.md), you will need to create a specific folder with a `mesg.yml` file that describes its functionalities. This file can contain the following information in a `YAML`syntax:

You can create a default file using the CLI by entering the command:

```bash
mesg-core service init
```

This will create a `mesg.yml` file in your current directory with the following attributes:

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- | --- | --- | --- |
| **name** | `""` | `String` | Each Service has a name chosen by the developer. This name is used to identify the service in a nice humanlike way. |
| **description** | `""` | `String` | A description that will be useful to explain the features of your service. |
| **publish** | `ALL` | [`Publish`](service-file.md#publish) | What is published on the network can be defined when deploying a Service by setting a publish parameter. [more](service-file.md#publish) |
| **visibility** | `ALL` | [`Visibility`](service-file.md#visibility) | A Service defines what entities can have access to the service. [more](service-file.md#visibility) |
| **events** | `{}` | `map<id,`[`Event`](communication-with-the-core.md)`>` | Services must declare a list of events they can emit. Events are actions on a technology the Service is connected to. |
| **tasks** | `{}` | `map<id,`[`Task`](tasks.md)`>` | Services declare a list of tasks they can execute. A task is an action that accepts parameters as inputs, executes something on the connected technology, and returns one output to Core, with data. |
| **dependencies** | `{}` | `map<id,`[`Dependency`](dockerize-your-service.md#add-your-dependencies)`>` | Services can specify internal dependencies such as a database, cache or blockchain client. |

You can find an example of `mesg.yml` file [here](https://github.com/mesg-foundation/service-ethereum/blob/master/mesg.yml).

### Publish

{% hint style="danger" %}
This feature is not yet implemented and will be implement when the network will be ready. 
{% endhint %}

What is published on the network can be defined by setting a publish parameter. The possible values are: `ALL`, `SOURCE`, `CONTAINER`, or `NONE`.

* `SOURCE` : only the source code will be published \(the developer will have to execute the service themselves\).
* `CONTAINER` : only the container will be published \(any worker will be able to run it\).
* `ALL` : both the source code and the container will be published.
* `NONE` : only the service config file will be published \(the developer will have to execute the service itself\).

### Visibility

{% hint style="danger" %}
This feature is not yet implemented and will be implement when the network will be ready.
{% endhint %}

A Service defines who can see it by setting a visibility parameter. The possible values are: `ALL`, `USERS`, `WORKERS`, or `NONE`.

* `USERS` : any user will be able to run the service.
* `WORKERS` : any worker will be able to run the service.
* `ALL` : any user and worker will be able to run the service.
* `NONE` : the service will only be accessible by manually entering its service ID \(which is created when deploying a service\).



