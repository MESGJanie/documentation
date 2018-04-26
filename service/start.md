# Run Your Service

## Start

To start a Service, run the command:

```bash
mesg-cli service start SERVICE_PATH
```

The command will download the Service's Docker container and will deploy it on your local computer. Once the Service is running, Core will register the Service on the Network.

Be aware that Services require computational power. Check the computational requirements of the Service before starting it. Also be sure to not run too many Services on a single computer to prevent your computer from lagging and causing a loss of stake because your services aren't running fast enough.

The command will ask you to specify the stake value and duration. If you wish to pass directly to the command, add the flags `--stake` and `--duration` :

```bash
mesg-cli service start SERVICE_ID --stake "100 MESG" --duration "10 days"
```

## Stop

**WARNING: **If you stop a service while your stake's duration is still ongoing, you may lose your stake. 

To stop a Service, run the command:

```bash
mesg-cli service stop SERVICE_ID
```

This command will stop the Docker container and unregister your node running the Service on the Network.

It will also remove the local Docker container from your disk to free up space.

You will **NOT** get your stake back immediately. You will get your remaining stake after a delay. 

