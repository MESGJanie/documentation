# Test {#testing}

Before deploying your service you want to test it to ensure that everything is working as expected.

By default, the `test `command looks in the **current folder** for the `mesg.yml` file. If you want to run it in a different folder, you can specify the path to the service folder like so:

```
mesg-cli service test ./PATH_TO_SERVICE_FOLDER
```

## Test service start

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

To test a task of your service, run:

```
mesg-cli service test --task myServiceTaskName
```

The Application will ask you to specify the `inputs` and `secrets` required by the task.

You can also provide the path to a JSON file containing the `inputs` and `secrets`:

```
mesg-cli service test --task myServiceTaskName ./PATH_TO_JSON_FILE.json
```

The JSON file should have a format like:

```
{
    "inputs": {
        "from": "0x0000000",
        "to": "0x0000001",
        "amount": 12345
    },
    "secrets": {
        "PRIVATE_KEY": "0x0000000"
    }
}
```

## Keep alive

All previous commands stop your service when the test is done. If you wish to leave your service alive, you can add to any previous commands the flag `--keep-alive.`Example:

```
mesg-cli service test --task myServiceTaskName --keep-alive
```



