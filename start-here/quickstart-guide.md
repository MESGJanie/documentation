# Quick Start Guide

**MESG is a platform for the creation of efficient and easy-to-maintain applications that connect any and all technologies.** 

### Contents

* [Quickstart](https://github.com/mesg-foundation/documentation/tree/e89cea583c4a219a5f6cad2b336139ce29ada953/start-here/installing-core/quickstart/README.md)
* [Service](https://github.com/mesg-foundation/documentation/tree/e89cea583c4a219a5f6cad2b336139ce29ada953/start-here/installing-core/service/README.md)
  * [Receiving Task](https://github.com/mesg-foundation/documentation/tree/e89cea583c4a219a5f6cad2b336139ce29ada953/start-here/installing-core/receiving-task/README.md)
  * [Submitting Event](https://github.com/mesg-foundation/documentation/tree/e89cea583c4a219a5f6cad2b336139ce29ada953/start-here/installing-core/submitting-event/README.md)
* [Architecture](https://github.com/mesg-foundation/documentation/tree/e89cea583c4a219a5f6cad2b336139ce29ada953/start-here/installing-core/architecture/README.md)
* [Examples](https://github.com/mesg-foundation/documentation/tree/e89cea583c4a219a5f6cad2b336139ce29ada953/start-here/installing-core/examples/README.md)
* [Roadmap](https://github.com/mesg-foundation/documentation/tree/e89cea583c4a219a5f6cad2b336139ce29ada953/start-here/installing-core/roadmap/README.md)

## Start Here

### **1 - Download the CLI**

First, download the CLI so you're able to interact with the MESG Core. You can either download the binaries directly from the [release page](https://github.com/mesg-foundation/core/releases/latest) then rename it to `mesg-core` and install it your path, or you can follow the installation process for your system: 

{% tabs %}
{% tab title="MacOS" %}
{% code-tabs %}
{% code-tabs-item title="install.sh" %}
```bash
curl https://github.com/mesg-foundation/core/releases/download/release-dev/mesg-core-darwin-10.6-amd64 --progress-bar -L -o ~/.local/bin/mesg-core
chmod +x ~/.local/bin/mesg-core
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="Linux" %}
{% code-tabs %}
{% code-tabs-item title="install.sh" %}
```bash
sudo curl https://github.com/mesg-foundation/core/releases/download/release-dev/mesg-core-linux-amd64 --progress-bar -L -o /usr/local/bin/mesg-core
sudo chmod +x /usr/local/bin/mesg-core
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

Then, to start Core, execute the following in a terminal:

```bash
mesg-core
```

{% hint style="warning" %}
If your system is not listed in the block before, please go to our [GitHub release page](https://github.com/mesg-foundation/core/releases) and download the correct one.
{% endhint %}

#### Docker CE

MESG also requires [Docker](https://www.docker.com/) to run on your machine.

If you don't have Docker installed on your machine yet, you can download it now from their official website:

* [Mac OS](https://store.docker.com/editions/community/docker-ce-desktop-mac)
* [Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)
* [Ubuntu](https://store.docker.com/editions/community/docker-ce-server-ubuntu)
* [Other](https://store.docker.com/search?type=edition&offering=community)

One this is done open a new terminal and type `mesg-core` and you should have something similar to this.

\[\[ TODO: Insert screenshot of the command line \]\]

### **2 - Run the MESG Daemon**

MESG needs to have a daemon running to process all the different commands that you might need to execute. In order to start the daemon you can run:

```text
mesg-core daemon start
```

### **3 - Deploy a service**

Next step is to deploy the service that your application will need. You can [create your own service](https://docs.mesg.tech/service/what-is-a-service), but for now, let's just use an existing one and deploy it.

```text
mesg-core deploy https://github.com/mesg-foundation/service-webhook
```

Let's deploy another one.

```text
mesg-core deploy https://github.com/mesg-foundation/service-invite-discord
```

Every time you deploy a service, the console will display the ID for the service you've just deployed.

### **4 - Connect the services**

Now, let's connect these services and create our application that will send you an email with an invitation to the MESG Discord every time you call the webhook.

```text
npm init && npm install --save mesg
```

Now create an `index.js` file and add the following code:

```javascript
const MESG = require('mesg/application')

const webhook    = '__ID_SERVICE_WEBHOOK__'
const invitation = '__ID_SERVICE_INVITATION_DISCORD__'
const email      = '__YOUR_EMAIL_HERE__'

MESG.ListenEvent({ serviceID: webhook, eventFilter: 'request' })
  .on('data', data => MESG.ExecuteTask({
    serviceID: invitation,
    taskKey: 'invite',
    taskData: JSON.stringify({ email })
  }, console.log))
```

Don't forget to replace the values `__ID_SERVICE_WEBHOOK__`, `__ID_SERVICE_INVITATION_DISCORD__` and `__YOUR_EMAIL_HERE__`.

### **5 - Start the application**

Start your application now like any node application:

```javascript
npm start
```

### **6 - Test the application**

Now we need to call the webhook in order to trigger the email, so let's do that with a curl command:

```text
curl -XPOST http://localhost:3000/webhook
```

You should now have an email in your inbox with your precious invitation to our Discord.

## Service

MESG depends heavily on services. These services are automatically built and ran inside Docker. You can connect anything you want, as long as it can run inside Docker \(as long as it can run on a computer\). If you need more details about how to connect dependencies to your service [check out the documentation](https://docs.mesg.tech/service/dockerize-the-service).

A service needs to implement two types of communications:

#### Receiving Tasks

Tasks are designed to receive information from Core and the Application that you run. Tasks can have multiple parameters as inputs and multiple outputs with varying data. You can visualize a task as a simple function that can return any kind of object.

You could have a task that takes a name as an input, and shows `success` as an output. This task factors the type of name with its probability like `{ "type": "female", "proabiliy": 92.34% }` but could also have an `error` output with a type of error like this `{ "message": "This doesn't looks like a name" }`.

Click here for more information on how info how to create [tasks](https://docs.mesg.tech/service/listen-for-tasks).

#### Submitting Events

Events are data that your service will emit in real time. Let's say you are doing a webserver. One event could be when there is a request with the data in the payload or different events for the different routes of your api or in a blockchain world when a smart contract emits an event.

More info how to create your [events in the documentation](https://docs.mesg.tech/service/emit-an-event)

## Architecture

\[\[ TODO: Add a nice graphic with the Application, the core and the services with the communication \]\]

## Examples

You can find a list of different examples and services that you can re-use [here](https://github.com/mesg-foundation/awesome)

## Roadmap

