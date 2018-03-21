# Stop a service

**WARNING: **If you stop a service with your stake duration still ongoing, you may lost your stake. See [stake explanation](/service/run/README.md).

To stop a service, run the command:

```bash
mesg-cli service stop SERVICE_ID
```

This command will stop the docker container and unregister from the Network that your node is running this service.

You will **NOT** get your stake back immediately. You will get your remaining stake only after a delay. See [stake explanation](/service/run/README.md).

**TODO:** add accountable system

**TODO:** add confirmable system

