# Run the Service

## Start

To start a Service, run the command:

```bash
mesg-core service start SERVICE_PATH
```

The command will validate that your Service is valid, then start the Docker container\(s\) needed to run your service.

## Stop

To stop a Service, run the command:

```bash
mesg-core service stop SERVICE_ID
```

This command will stop the Docker container\(s\) started for your service.

## Status

You can check all the services that are running anytime by running the following command :

```bash
mesg-core service status
```

