# Dockerize your service

## Why do I need Docker ?

In MESG all the services run in Docker to provide a sandbox for your service to run without any problems and side effects that might happen in a local machine like your service not compatible on Windows or Linux or this kind of  things that can be really complicated to achieve and also make sure that you can have all the time the exact same environment for your service to run.

## How can I be compatible with Docker ?

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



