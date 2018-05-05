# Dockerize the service

## Why do I need Docker ?

Services run in Docker to provide sandbox and normalized environment to remove side effects that happen when running on many different machines. See more information on the [Docker website](https://www.docker.com/).

## Steps to be compatible with Docker

* [ ] [Create the Dockerfile](dockerize-your-service.md#create-your-dockerfile)
* [ ] [Add dependencies](dockerize-your-service.md#add-dependencies) in your [`mesg.yml`](service-file.md) file

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
{% endcode-tabs %}

[source](https://blog.codeship.com/building-minimal-docker-containers-for-go-applications/)
{% endtab %}
{% endtabs %}

## Add dependencies

Once the service can run on Docker, the [Core](../start-here/core.md) should be able to start it automatically. Update the [`mesg.yml`](service-file.md) file with the dependencies the service needs.

{% hint style="warning" %}
The service always needs to be declared as a dependency.
{% endhint %}

{% tabs %}
{% tab title="Detail" %}
| **Attribute** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **image** | `String` | The docker image of the service. |
| **volumes** | `array[string]` | A list of [volumes](https://docs.docker.com/storage/volumes/) that will be mounted in the service. |
| **ports** | `array[string]` | A list of ports that the service needs to expose. |
| **command** | `String` | The command to run when the service starts if not defined in your [Dockerfile](dockerize-your-service.md#create-your-dockerfile). |
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



