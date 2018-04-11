# Stop

**TO DELETE**

**WARNING: **If you stop a service with your stake duration still ongoing, you may lost your stake. See [stake explanation](https://github.com/mesg-foundation/documentation/tree/b3d92737e4dfd41f30e20d0ab1f2b8dbbf045a2d/service/run/README.md).

To stop a service, run the command:

```bash
mesg-cli service stop SERVICE_ID
```

This command will stop the docker container and unregister from the Network that your node is running this service.

It will also remove the local docker container from your disk to free space.

You will **NOT** get your stake back immediately. You will get your remaining stake only after a delay. See [stake explanation](https://github.com/mesg-foundation/documentation/tree/b3d92737e4dfd41f30e20d0ab1f2b8dbbf045a2d/service/run/README.md).

