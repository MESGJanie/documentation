# Service file

## Service file

To define a [Service](what-is-a-service.md), you will need to create a specific folder with a `mesg.yml` file that describes its functionalities. This file can contain the following information in a `YAML`syntax:

### Name

Each Service has a name chosen by the developer. This name is used to identify the service in a nice humanlike way.

### Identifiers

Each Service has two unique identifiers: a Service ID and Version ID. If necessary, both will be generated during deployment. 

* The Service ID stays the same for the entire life of the Service. 
* A Version ID is generated for each new deployment. 

Both IDs are unique across all services. The Service ID can be used to manually select a private service.

### Computational requirements

Services can specify their CPU, memory and disk requirements. Core will ensure the requirements are compatible with the machine it's running on.

### Visibility

A Service defines who can see it by setting a visibility parameter. The possible values are: **all**, **users**, **workers**, or **none**. 

* If set to "**users**", any user will be able to run the service. 
* If set to "**workers**", any worker will be able to run the service.
* If set to "**all**", any user and worker will be able to run the service. 
* If set to "**none**", the service will only be accessible by manually entering its service ID \(which is created when deploying a service\).

### Publish

What is published on the network can be defined when deploying a Service by setting a publish parameter. The possible values are: **all**, **source**, **container**, or **none**.

* If set to "**source**", only source code will be published \(the developer will have to execute the service themselves\). 
* If set to "**container**", only the container will be published \(any worker will be able to run it\). 
* If set to "**all**", both the source code and the container will be published. 
* If set to "**none**", only the service config file will be published \(the developer will have to execute the service itself\).

### Events

Services must declare a list of events they can emit. Events are actions on a technology the Service is connected to.  
  
Each event is composed of the following:

* **Identifier**: The unique identifier for this event.
* **Name**: The name for the event.
* **Description**: A description for the event \(optional\).
* **Data**: A list of data associated with this event including the following information:
  * **Identifier**: The unique identifier for this data
  * **Name**: The name of the data
  * **Type**: The type of data
  * **Description**: An optional description for the data
  * **Optional**: A boolean to say if the data is optional or not

### Tasks

Services declare a list of tasks they can execute. A task is an action that accepts parameters as inputs, executes something on the connected technology, and returns one output to Core, with data.

#### Name

The name of the task.

#### Inputs

List of inputs needed for the task, every input has the structure :

* **Identifier**: The unique identifier for this input
* **Name**: A name for the input \(optional\)
* **Type**: The type of input
* **Description**: A description for the input \(optional\)
* **Optional**: A boolean to say if this input is optional or not

#### Outputs

Tasks can have multiple outputs, but only one can be returned for each execution. There is a list of different outputs for tasks, and every output has the structure:

* **Identifier**: The unique identifier for this output
* **Name**: A name for the output \(optional\)
* **Description**: A description for the output \(optional\)
* **Data**: A list of data with the following structure:
  * **Identifier**: The unique identifier for the data
  * **Name**: The name of the data
  * **Description**: A description for the data \(optional\)
  * **Type**: The type of data
  * **Optional**: A boolean to say if the data is optional or not

#### Secrets

A list of secrets is needed for a Service to run. Secrets are stored directly on the Node and may differ between Nodes of the Network.

* **Identifier**: A unique identifier for the secret
* **Name**: A name for the secret \(optional\) 
* **Description**: A description for the secret \(optional\)
* **Type**: The type of secret

#### Verification

If the task is verifiable, a verifiable function should be implemented in order to ensure that an output is correct according to the inputs. This is useful to verify that a node has correctly executed a task by asking other nodes for verification.

### Dependencies

Services can specify internal dependencies such as a database, cache or blockchain client.  
  
If Docker is used you can use the Docker-Compose syntax.

## [Example of a service file](https://github.com/mesg-foundation/documentation/blob/master/service/mesg.yml)

