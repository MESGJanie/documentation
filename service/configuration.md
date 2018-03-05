## Name

A service has a name chosen by the developer. This name is for user to identify the service is a nice human way.

## Identifiers

Each service have 2 unique identifiers: service ID and version ID. Both are generated (if needed) during the deployment. The service ID stays the same for all the life of the service. The version ID is generated for every new deployment. Both IDs are unique across all services. The service ID can be use by users to manually choose a private service (see Visibility).

## Computation requirements

Services can specify its cpu, memory and disk requirements. The Application will check if the requirements are compatible with the machine it is running on.

## Events

Services have to declare a list of events they can emit. Events are things that happen on the technology the service is connected to.
Each event is composed with:

- Identifier: The unique identifier for this event.
- Name: Event name.
- Description: An optional description for the event.
- Data: A list of data associated to this event that have the following informations:
  - Identifier: The unique identifier for this data
  - Name: The name of the data
  - Type: The type of the data
  - Description: An optional description for the data
  - Optional: A boolean to say if this data is optional or no

## Tasks

Services declares a list of tasks they can execute. A task is an action that accept parameters as inputs, execute something on the technology the service is connected to, and returns one output from many with data.

#### Name

Name of the task

#### Inputs

List of inputs needed for the task, every input have the structure :

- Identifier: The unique identifier for this input
- Name: An optional name for the input
- Type: The type of the input
- Description: An optional description for the input
- Optional: A boolean to say if the input is optional or no
  
#### Outputs

A tasks can have multiple outputs but only one can be returned for each execution
List of the different outputs for the task, every output have the structure:

- Identifier: The unique identifier for this output
- Name: An optional name for the output
- Description: An optional description for the output
- Data: A list of data with the following structure:
  - Identifier: The unique identifier for this data
  - Name: The name of this data
  - Description: An optional description for this data
  - Type: The type of this data
  - Optional: A boolean to say if this data is optional or no
  
#### Secrets

List of the secrets needed for the service to run. Those secrets are stored directly on the Node and might be different between Nodes of the Network.

- Identifier: The unique identifier for this secret
- Name: An optional name for the secret
- Description: An optional description for the secret
- Type: The type of this secret

#### Verifiable

If the task is verifiable, a verifiable function should be implemented in order to check that an output is correct according to the inputs. This is really useful in order to verify that a node has correctly executed a task by asking other nodes to verify it.


## Transfers

A service can implement the network functionality of transfers (see Transfers for more explanation).
The implementation is composed of a require action and an optional verification event.

Action

The action is required and has predefined parameters:

- Transfer ID: the transfer ID of this transfer.
- Inputs: a payload of parameters the service defines. Same structure as Task Inputs.
- Secrets: a payload of secrets the service defines. Same structure as Task Secrets.

The result is also required and predefined. It can be either:

- Success
  - Message
- Error
  - Message

Verification

If the transaction on this technology is verifiable, the service emits a predefined event that has a structure of:

- Transfer ID: the transfer ID of that was encoded in the transaction
- Success: Boolean, yes if success, false otherwise.
- Message: Reason of failure or confirmation


## Dependencies

Services can specify internal dependencies like database, cache or blockchain client.
If docker is used we could use the docker-compose syntax.


## Visibility

A service defines who can see it by setting a visibility parameter. The possible values are: all, users, workers, none. If set to users, any user will be able to select the service in a task. If set to workers, any worker will be able to run the service. If set to all, any user and worker will be able to access it. If set to none, the service will be accessible by manually entering its service ID (that will be creating when deploying the service).


## Publish

A service defines what is published on the network when deploying by setting a publish parameter. The possible values are: all, source, container, none. If set to source, only the source code will be published (the developer will have to execute the service itself). If set to container, only the container will be published (any worker will be able to run it). If set to all, both the source code and the container will be published. If set to none, only the service config file will be published (the developer will have to execute the service itself).

# Example of service
[include](./example.yml)

