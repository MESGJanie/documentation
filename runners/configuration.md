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

[include](./example.yml)

