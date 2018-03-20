# Test {#testing}

Before deploying your service you want to test it to ensure that everything is working as expected.

The `test `command have different options but always requires the path to the service file you want to test.

## Test container/docker

To test that the Application is able to start your service, run the command:

```
mesg-cli service test service.yml
```

This command will start your service \(the docker container of your service\), wait for your service to emit the `started` systen event, and then stop it.

## Test events

To 

```
mesg-cli service test service.yml --event
```

If you want to try your workflow with a specific event you can run the following command:

```
mesg-cli service test service.yml --event myServiceEventName
```

## Test a task

When you want to test that your workflow is working fine with any events you are trying to connect to you can run the following command:

```
mesg-cli service test --live workflow.yml
```

Test a

