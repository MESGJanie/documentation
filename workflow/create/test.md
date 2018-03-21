# Test

Before deploying your workflow you want to test it to ensure that everything is working as expected.

When you run tests, all services the workflow is using will be started on your computer.

> We recommend to test with a live event to have valid data. You can then use the previous live event to retry the workflow if you need to debug it.

## Test with a live event

The `test` command have different options but you always need to specify the path to the workflow file you want to test.

Using the `test` command, the source's service will run additionally to the task's services so this might take some resource from your computer.

During the testing, no stake will be taken in order to start the services and the services you will be running will not participate to the network.

#### Test a specific event

If you want to try your workflow with a specific event you can run the following command:

```bash
mesg-cli workflow test --event event.json ./workflow.yml
```

#### Test a live event

When you want to test that your workflow is working fine with any events you are trying to connect to you can run the following command:

```bash
mesg-cli workflow test --live ./workflow.yml
```

**Note**: Every live event will be stored as a file and can be re-use as an event flag

