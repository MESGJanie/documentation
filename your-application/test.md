# Test

Before deploying your workflow, test it to ensure that everything is working as expected.

When you run tests, all of the services a workflow is using will be started on your computer.

> We recommend to test with a live event to have valid data. You can then use the previous live event to retry the workflow if you need to debug it.

## Test with a live event

The `test` command has different options, but you'll always need to specify the path to the workflow file that you want to test.

Using the `test` command, the source's service will run in addition to the task's services, so it may use some resources from your computer.

During testing, no stake will be taken in order to start the services, and the services you will be running will not participate in the network.

### Test a specific event

If you want to try your workflow with a specific event you can run the following command:

```bash
mesg-core workflow test --event ./PATH_TO_EVENT_DATA_FILE ./PATH_TO_WORKFLOW_FILE
```

### Test a live event

When you want to test that your workflow is working fine with any events you are trying to connect to you can run the following command:

```bash
mesg-core workflow test --live ./PATH_TO_WORKFLOW_FILE
```

**Note**: Every live event will be stored as a file and can be re-used as an event flag

**TODO:** add specific task test. May have to update the CLI doc as well

