# Dockerize the Service

## Why do I need Docker ?

Services run in Docker to provide a sandbox and a normalized environment to remove any side effects that may occur when running on many different machines. See more information on the [Docker website](https://www.docker.com/).

## Steps to be compatible with Docker

* [ ] [Create the Dockerfile](dockerize-the-service.md#create-your-dockerfile)
* [ ] [Add dependencies](dockerize-the-service.md#add-dependencies) in your [`mesg.yml`](service-file.md) file

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

Once the Service can run on Docker, [Core](../start-here/core.md) should be able to start it automatically. Update the [`mesg.yml`](service-file.md) file with the dependencies the service needs.

{% hint style="warning" %}
The Service always needs to be declared as a dependency.
{% endhint %}

{% tabs %}
{% tab title="Detail" %}
| **Attribute** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **image** | `String` | The docker image of the Service. |
| **volumes** | `array[string]` | A list of [volumes](https://docs.docker.com/storage/volumes/) that will be mounted in the Service. |
| **ports** | `array[string]` | A list of ports that the Service needs to expose. |
| **command** | `String` | The command to run when the Service starts if not defined in your [Dockerfile](dockerize-the-service.md#create-your-dockerfile). |
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



