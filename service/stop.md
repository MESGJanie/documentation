# Stop

**TO DELETE**

**WARNING: **If you stop a service while your stake duration is still ongoing, you may lose your stake. See [stake explanation](../start-here/what-is-a-node.md#stake).

To stop a service, run the command:

```bash
mesg-cli service stop SERVICE_ID
```

This command will stop the docker container and unregister your node running the service from the Network.

It will also remove the local docker container from your disk to free up space.

You will **NOT** get your stake back immediately. You will get your remaining stake after a delay. See the [stake explanation](../start-here/what-is-a-node.md#stake).

