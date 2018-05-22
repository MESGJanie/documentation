# Publish a service

Once you finish developing your service and testing it, you can publish it. A unique ID will be generated when you publish a service. This ID is based on the mesg.yml file and will change every time you add any modifications to this service.

To publish the service you can run the command:

```bash
mesg-core service publish PATH_OF_THE_SERVICE
```

This will generate an ID that looks similar to v1\_fe25be776e1e256400c77067a1cb7666. You can reuse this ID when you want to use the service.

### List published services

If you want to see the list of services already published you can run the command:

```bash
mesg-core service list
```

### Delete a published service

If for any reason you want to delete a service that you previously published you can do it using the command:

```bash
mesg-core service delete SERVICE_ID
```



