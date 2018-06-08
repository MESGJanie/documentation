# Quick Start Guide

**MESG is a platform for the creation of efficient and easy-to-maintain applications that connect any and all technologies.**

## Start Here

### **Download the CLI**

First, download the CLI so you're able to interact with the MESG Core. You can either download the binaries directly from the [release page](https://github.com/mesg-foundation/core/releases/latest) then rename it to `mesg-core` and install it your path, or you can follow the installation process for your system:

### **Run MESG Core**

MESG needs to have a daemon running to process all the different commands that you might need to execute. In order to start the daemon you can run:

```text
mesg-core start
```

### **Deploy a service**

Next step is to deploy the service that your application will need. You can [create your own service](https://docs.mesg.tech/service/what-is-a-service), but for now, let's just use an existing one and deploy it.

```text
mesg-core deploy https://github.com/mesg-foundation/service-webhook
```

Let's deploy another one.

```text
mesg-core deploy https://github.com/mesg-foundation/service-invite-discord
```

Every time you deploy a service, the console will display the ID for the service you've just deployed.

### **Connect the services**

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

### **Start the application**

Start your application now like any node application:

```javascript
npm start
```

### **Test the application**

Now we need to call the webhook in order to trigger the email, so let's do that with a curl command:

```text
curl -XPOST http://localhost:3000/webhook
```

You should now have an email in your inbox with your precious invitation to our Discord.

