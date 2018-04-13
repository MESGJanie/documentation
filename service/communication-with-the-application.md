# Communication with the Application

## Receiving commands from the Application

The Service needs to receive the command sent by the Application. Every time a command is received, it will make sure that the sender is the Application, then check that it can handle the command, and if so, execute it. Once executed, it will emit an event to the Application with the result of the command.

**TODO: a technical definition**

## Send events to the Application

The Service can send data to the Application. The data is divided into three categories:

* Result of a command: When the Application asks to start a command and expects a result from this command
* Events from listener: When the Service emits a new event from its listener function. \(Eg: web server running and receiving a request or a blockchain technology that received a new transaction\)
* System events: When the service is ready to be executed, notify the Application

**TODO: a technical definition**

