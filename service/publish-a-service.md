# Publish a service

Once you finished to develop your service and test it you can now publish it. When publishing the service, an unique ID will be generated, this ID is based on the [`mesg.yml`](service-file.md) file and will change every time you add any modification on this service.

To publish the service you can run the command:

```bash
mesg-core service publish PATH_OF_THE_SERVICE
```

This will return an ID that looks similar to `v1_fe25be776e1e256400c77067a1cb7666`. You will be able to reuse this ID when you want to use the service.

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



