# Start a service

To start a service, run the command:

```bash
mesg-cli service start SERVICE_ID
```

This command will download the docker container of the service and deploy it on your local computer. Once the service is up and running, the Application will register on the Network your are running this service.

Be aware that services require computation power. Check the computation required of this service before starting it. Also don't run too many services on a single computer to prevent your computer to lag and cause loss of stake because your services will not run fast enough.

The command will ask you to specify the stake value and duration. If you wish to pass them directly to the command, add the flags `--stake` and `--duration` :

```bash
mesg-cli service start SERVICE_ID --stake "100 MESG" --duration "10 days"
```

**TODO:** add unit description page
