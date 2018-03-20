# Test {#testing}

Before deploying your service you want to test it to ensure that everything is working as expected.

By default, the `test `command looks in the current folder for the `mesg.yml` file. If you want to run it in a different folder, you can specify the path to the service folder:

```
mesg-cli service test ./PATH_TO_SERVICE_FOLDER
```

## Test start service

To test that the Application is able to start your service, simply run the command:

```
mesg-cli service test
```

This command will start your service \(the docker container of your service\), wait for your service to emit the `started` system event, and then stop it.

## Test events

To start your service and make the Application listen to an event, run:

```
mesg-cli service test --event myServiceEventName
```

To listen to **all** events the service can emit, run:

```
mesg-cli service test --event *
```

## Test a task

When you want to test that your workflow is working fine with any events you are trying to connect to you can run the following command:

```
mesg-cli service test --live workflow.yml
```

Test a

