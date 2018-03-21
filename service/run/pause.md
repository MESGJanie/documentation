# Pause a service

To pause a service, run the command:

```bash
mesg-cli service pause SERVICE_ID
```

This command will stop the docker container, unregister from the Network that your node is running this service.

You will **NOT** get your stake back immediately. You will get your remaining stake only after a delay. See [stake explanation](/service/run/README.md).

**TODO:** add accountable system

**TODO:** add confirmable system

