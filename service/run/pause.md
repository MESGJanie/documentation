# Pause a service

To pause a service, run the command:

```bash
mesg-cli service pause SERVICE_ID
```

This command will stop the docker container and tell the Network that your node has paused this service.

You will **NOT** get your stake back with this command. The goal of this command is to give you an opportunity to stop running a service for a short period of time without losing your stake.

When a service is paused, the stake duration count is also paused.

When you tell the Application to quit, it will run this command for every started service.

**TODO:** add accountable system

**TODO:** add confirmable system

