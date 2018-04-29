# Dockerize your service

## Why do I need Docker ?

In MESG all the services run in Docker to provide a sandbox for your service to run without any problems and side effects that might happen in a local machine like your service not compatible on Windows or Linux or this kind of  things that can be really complicated to achieve and also make sure that you can have all the time the exact same environment for your service to run.

### Steps to be compatible with Docker

* [ ] [Create a Dockerfile](dockerize-your-service.md#create-your-dockerfile) to be compatible with Docker
* [ ] [Add your dependencies](dockerize-your-service.md#add-your-dependencies) in your [`mesg.yml`](service-file.md) file

## Create your Dockerfile ?

In order to be compatible with [Docker](https://www.docker.com/) you need in the folder of your application to create a `Dockerfile` file which is a file that creates all the environment for your [Docker container](https://www.docker.com/what-container).

You can find more details on how to create your `Dockerfile` [here](https://docs.docker.com/engine/reference/builder/).

Here are some exemples of `Dockerfile` that you might use for your application.

{% tabs %}
{% tab title="Node" %}
{% code-tabs %}
{% code-tabs-item title="Dockerfile" %}
```bash
FROM node:carbon
WORKDIR /usr/src/app
COPY package*.json ./
RUN npm install
COPY . .
EXPOSE 8080
CMD [ "npm", "start" ]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

[source](https://nodejs.org/en/docs/guides/nodejs-docker-webapp/)
{% endtab %}

{% tab title="Go" %}
{% code-tabs %}
{% code-tabs-item title="Dockerfile" %}
```bash
FROM golang:latest 
RUN mkdir /app 
ADD . /app/ 
WORKDIR /app 
RUN go build -o main . 
CMD ["/app/main"]
```
{% endcode-tabs-item %}
{% endcode-tabs %}

[source](https://blog.codeship.com/building-minimal-docker-containers-for-go-applications/)
{% endtab %}
{% endtabs %}

## Add your dependencies

Once your application can run on Docker you need to make sure that MESG [core](../start-here/core.md) will be able to start it for you. In order to do that you need to update your [`mesg.yml`](service-file.md) file with all the dependencies that you need for your service to run efficiently.

{% hint style="info" %}
**Note:** You always need to put your service as a dependency otherwise your service cannot run.
{% endhint %}

{% tabs %}
{% tab title="Detail" %}
| **Attribute** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **image** | `String` | The docker image of your service |
| **volumes** | `array[string]` | A list of [volumes](https://docs.docker.com/storage/volumes/) that will be mounted in your service |
| **ports** | `array[string]` | A list of ports that your service needs to listen |
| **command** | `String` | The command to run when your service starts if not defined in your [Dockerfile](dockerize-your-service.md#create-your-dockerfile) |
{% endtab %}

{% tab title="Example" %}
{% code-tabs %}
{% code-tabs-item title="mesg.yml" %}
```yaml
name: function
description: execute a function
visibility: ALL
publish: ALL
tasks: {}
events: 
  eventX:
    name: "Event X"
    data:
      foo:
        name: "Foo"
        type: String
      bar:
        name: "Bar"
        type: Boolean
tasks:
  taskX:
    name: "Task X"
    description: "This is the task X"
    inputs:
      foo:
        name: "Foo"
        description: "Foo is the first data"
        type: String
        optional: false
      bar:
        name: "Bar"
        description: "Bar is the second data"
        type: Boolean
        optional: true
    outputs:
      outputX:
        name: "OutputX"
        description: "Output X"
        data:
          outputDataX:
            name: "Output data x"
            description: "Description about output data x"
            type: String
          outputDataY:
            name: "Output data y"
            description: "Description about output data y"
            type: Number
    secrets:
      SECRET_X:
        name: "SecretX"
        type: String
dependencies:
  function:
    image: mesg/function
    volumes: []
    ports: []
    command: ""

```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



