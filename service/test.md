# Test {#testing}

Before deploying your service you want to test it to ensure that everything is working as expected.

To test that the Application is able to start your service and receive event from your service, simply run the command:

```bash
mesg-cli service test
```

This command will start your service \(the docker container of your service\), wait for your service to emit the `started` system event, and then will log any event the Application is receiving from your service. To quit, press `ctrl+c`.

By default, the `test`command looks in the **current folder** for the `mesg.yml` file. If you want to run it in a different folder, you can specify the path to your service folder at the end of any command like so:

```bash
mesg-cli service test ./PATH_TO_SERVICE_FOLDER
```

## Listen to an event

To only listen to one event, you can specify the name of the event with the `--event` flag:

```bash
mesg-cli service test --event myServiceEventName
```

## Run a task

To test a task of your service, run:

```bash
mesg-cli service test --task myServiceTaskName
```

The Application may ask you to specify the `inputs` and `secrets` required by the task.

You can also provide the path to a file containing the `inputs` and `secrets`:

```bash
mesg-cli service test --task myServiceTaskName --data ./PATH_TO_DATA_FILE.yml
```

The file should be a `yaml`with a format like:

```yml
inputs:
    INPUT_NAME_1: INPUT_VALUE_1
    INPUT_NAME_2: INPUT_VALUE_2
secrets:
    SECRET_NAME_1: SECRET_VALUE_1
```

## Keep alive

All previous commands stop your service on quit. If you wish to leave your service alive, you can add to any commands the flag `--keep-alive.`Example:

```bash
mesg-cli service test --task myServiceTaskName --keep-alive
```



