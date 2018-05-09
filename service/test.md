# Test

Before deploying the Service, you'll want to test it to ensure that everything is working properly.

To ensure that the Application is able to start your Service and receive an event from it, execute the following method:

```bash
mesg-core service test ./PATH_TO_SERVICE_FOLDER
```

If you don't specify the path to the service folder, the method searches in the current folder for the `mesg.yml` file.

## Listen to a specific event

To only listen to one specific event, you can specify the name of the event with the `--event` flag:

```bash
mesg-core service test --event myServiceEventName
```

## Run a task

To test a task from your Service, run:

```bash
mesg-core service test --task myServiceTaskName
```

If your task requires inputs you will need to specify the file that contains all the input values in `yml` format.

```bash
mesg-core service test --task myServiceTaskName --inputs ./PATH_TO_INPUTS_FILE.yml
```

The file should be a `yaml`with a format similar to the following:

```yaml
inputX: "..."
inputY: "..."
```

## Keep it alive

All previous commands will stop your service upon quitting. If you want to leave your service alive, you can add the following flag to any command: `--keep-alive`. For example:

```bash
mesg-core service test --task myServiceTaskName --keep-alive
```



