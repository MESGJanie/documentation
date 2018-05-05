# Dockerize the service

## Why do I need Docker ?

Services run in Docker to provide sandbox and normalized environment to remove side effects that happen when running on many different machines. See more information on the [Docker website](https://www.docker.com/).

## Steps to be compatible with Docker

* [ ] [Create the Dockerfile](dockerize-your-service.md#create-your-dockerfile) to be compatible with Docker
* [ ] [Add dependencies](dockerize-your-service.md#add-your-dependencies) in your [`mesg.yml`](service-file.md) file

## Create the Dockerfile

In order to be compatible with [Docker](https://www.docker.com/), a `Dockerfile` needs to be created in the folder of the service. See the [Dockerfile reference](https://docs.docker.com/engine/reference/builder/).

### Examples

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

{% code-tabs-item title=undefined %}
```
Dockerfile
```
{% endcode-tabs-item %}
{% endcode-tabs %}

[source](https://blog.codeship.com/building-minimal-docker-containers-for-go-applications/)
{% endtab %}
{% endtabs %}

## Add your dependencies

Once your application can run on Docker, you need to make sure that MESG [Core](../start-here/core.md) will be able to start it for you. In order to do that, you need to update your [`mesg.yml`](service-file.md) file with all the dependencies you need for your service to run efficiently.

{% hint style="info" %}
**Note:** You always need to use your service as a dependency, otherwise your service cannot run.
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
name: serviceX
tasks: {}
events: {}
dependencies:
  service:
    image: "serviceXImage"
    command: "node start"
  serviceToConnectWith:
    image: "..."
    volumes:
      - "/tmp"
    ports:
      - "1234"
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



