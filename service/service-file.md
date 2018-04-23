# Service file

## Service file

To define a service, you need to create a specific folder with a `mesg.yml` file that describes its functionalities. This file should/may contain the following information in a `YAML`syntax.

### Name

A service has a name chosen by the developer. This name is for the user to identify the service in a nice human way.

### Identifiers

Each service have 2 unique identifiers: service ID and version ID. Both are generated \(if needed\) during deployment. The service ID stays the same for the entire life of the service. A version ID is generated for each new deployment. Both IDs are unique across all services. The service ID can be used by users to manually choose a private service \(see Visibility\).

### Computation requirements

Services can specify its cpu, memory and disk requirements. The Application will check if the requirements are compatible with the machine it's running on.

### Visibility

A service defines who can see it by setting a visibility parameter. The possible values are: all, users, workers, or none. If it is set to "users", any user will be able to select the service in a task. If set to "workers", any worker will be able to run the service. If set to "all", any user and worker will be able to access it. If set to "none", the service will be accessible by manually entering its service ID \(that is created when deploying a service\).

### Publish

A service defines what's published on the network when deploying by setting a publish parameter. The possible values are: all, source, container, none. If set to "source", only source code will be published \(the developer will have to execute the service themselves\). If set to "container", only the container will be published \(any worker will be able to run it\). If set to "all", both the source code and the container will be published. If set to "none", only the service config file will be published \(the developer will have to execute the service itself\).

### Events

Services have to declare a list of events they can emit. Events are things that happen on the technology the service is connected to.  
Each event is composed with:

* Identifier: The unique identifier for this event.
* Name: Event name.
* Description: An optional description for the event.
* Data: A list of data associated with this event that has the following information:
  * Identifier: The unique identifier for this data
  * Name: The name of the data
  * Type: The type of the data
  * Description: An optional description for the data
  * Optional: A boolean to say if this data is optional or not

### Tasks

Services declare a list of tasks they can execute. A task is an action that accepts parameters as inputs, executes something on the technology the service it's connected to, and returns one output from many with data.

#### Name

Name of the task

#### Inputs

List of inputs needed for the task, every input has the structure :

* Identifier: The unique identifier for this input
* Name: An optional name for the input
* Type: The type of input
* Description: An optional description for the input
* Optional: A boolean to say if the input is optional or not

#### Outputs

Tasks can have multiple outputs but only one can be returned for each execution.  
There is a list of different outputs for tasks, and every output has the structure:

* Identifier: The unique identifier for this output
* Name: An optional name for the output
* Description: An optional description for the output
* Data: A list of data with the following structure:
  * Identifier: The unique identifier for the data
  * Name: The name of the data
  * Description: An optional description for the data
  * Type: The type of data
  * Optional: A boolean to say if the data is optional or not

#### Secrets

There is a list of the secrets needed for the service to run. Those secrets are stored directly on the Node and might be different between Nodes of the Network.

* Identifier: The unique identifier for this secret
* Name: An optional name for the secret
* Description: An optional description for the secret
* Type: The type of this secret

#### Verifiable

If the task is verifiable, a verifiable function should be implemented in order to check that an output is correct according to the inputs. This is really useful in order to verify that a node has correctly executed a task by asking other nodes to verify it.

### Transfers

A service can implement the network functionality of transfers.  
The implementation is composed of a required action and an optional verification event.

Action

The action is required and has predefined parameters:

* Transfer ID: the transfer ID of this transfer.
* Inputs: a payload of parameters the service defines. Same structure as Task Inputs.
* Secrets: a payload of secrets the service defines. Same structure as Task Secrets.

The result is also required and predefined. It can be either:

* Success
  * Message
* Error
  * Message

Verification

If the transaction on this technology is verifiable, the service emits a predefined event that has a structure of:

* Transfer ID: the transfer ID that was encoded in the transaction
* Success: Boolean, yes if successful, and no otherwise.
* Message: Reason of failure, or confirmation

### Dependencies

Services can specify internal dependencies like a database, cache or blockchain client.  
If docker is used we can use the docker-compose syntax.

## [Example of a service file](https://github.com/mesg-foundation/documentation/blob/master/service/mesg.yml)

