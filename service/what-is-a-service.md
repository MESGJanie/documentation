# What is a Service?

A Service is a normalized bi-directional communication layer, which acts a connection from any technology to Core.   

MESG depends heavily on services. These services are automatically built and ran inside Docker. You can connect anything you want, as long as it can run inside Docker \(as long as it can run on a computer\). If you need more details about how to connect dependencies to your service [check out the documentation](https://docs.mesg.tech/service/dockerize-the-service).

A service needs to implement two types of communications:

#### Receiving Tasks

Tasks are designed to receive information from Core and the Application that you run. Tasks can have multiple parameters as inputs and multiple outputs with varying data. You can visualize a task as a simple function that can return any kind of object.

You could have a task that takes a name as an input, and shows `success` as an output. This task factors the type of name with its probability like `{ "type": "female", "proabiliy": 92.34% }` but could also have an `error` output with a type of error like this `{ "message": "This doesn't looks like a name" }`.

Click here for more information on how info how to create [tasks](https://docs.mesg.tech/service/listen-for-tasks).

#### Submitting Events

Events are data that your service will emit in real time. Let's say you are doing a webserver. One event could be when there is a request with the data in the payload or different events for the different routes of your api or in a blockchain world when a smart contract emits an event.

More info how to create your [events in the documentation](https://docs.mesg.tech/service/emit-an-event)


**In Q3 2018**, once the Network is deployed, Services can be shared or sold on our Marketplace, allowing users to use any technology without needing to code. However, for the time being, developers will need to create their own Services or share them on GitHub. 
