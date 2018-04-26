# Run Your Service

## Start

To start a service, run the command:

```bash
mesg-cli service start SERVICE_PATH
```

This command will download the docker container of the service and deploy it on your local computer. Once the service is up and running, the Application will register on the Network you're running the service on.

Be aware that services require computational power. Check the computation required of this service before starting it. Also don't run too many services on a single computer to prevent your computer to lag and cause loss of stake because your services won't run fast enough.

The command will ask you to specify the stake value and duration. If you wish to pass them directly to the command, add the flags `--stake` and `--duration` :

```bash
mesg-cli service start SERVICE_ID --stake "100 MESG" --duration "10 days"
```

## Stop

**WARNING: **If you stop a service while your stake duration is still ongoing, you may lose your stake. 

To stop a service, run the command:

```bash
mesg-cli service stop SERVICE_ID
```

This command will stop the docker container and unregister your node running the service from the Network.

It will also remove the local docker container from your disk to free up space.

You will **NOT** get your stake back immediately. You will get your remaining stake after a delay. 

