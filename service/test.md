# Test

Before deploying your Service, you'll want to test it to ensure that everything is working properly.

To ensure that the Application is able to start your Service and receive an event from it, execute the following method:

```bash
mesg-cli service test ./PATH_TO_SERVICE_FOLDER
```

If you don't specify the path to the service folder, the method searches in the current folder for the `mesg.yml` file.

## Listen to an event

To only listen to one event, you can specify the name of the event with the `--event` flag:

```bash
mesg-cli service test --event myServiceEventName
```

## Run a task

To test a task from your service, run:

```bash
mesg-cli service test --task myServiceTaskName
```

The Application may ask you to specify the `inputs` and `secrets` required by the task.

You can also provide the path to a file containing the `inputs` and `secrets`:

```bash
mesg-cli service test --task myServiceTaskName --data ./PATH_TO_DATA_FILE.yml
```

The file should be a `yaml`with a format like:

```text
inputs:
    INPUT_NAME_1: INPUT_VALUE_1
    INPUT_NAME_2: INPUT_VALUE_2
secrets:
    SECRET_NAME_1: SECRET_VALUE_1
```

## Keep it alive

All previous commands stop your service upon quitting. If you wish to leave your service alive, you can add the following flag to any command: `--keep-alive`. For example:

```bash
mesg-cli service test --task myServiceTaskName --keep-alive
```



