# Testing

Before deploying your worflow you might want to test it to ensure that everything is working as expected.

Every time you run some test all the services that you need to execute will be executed on your computer and this service will also be available for the rest of {{ book.network }} so your computer might process some other tasks.

> We recommand to test with a live event in order to have some valid data and then use the previous results from the live event to retry with the custom event if you need to debug your workflow.

## Testing with a live event

The `test` command have different options and always have the workflow file you want to test.

Using the `test` command, the source's service will run aditionnaly to the task's services so this might take some ressource from your computed.

During the testing, no stake will be taken in order to start the services and the services you will be running will not participate to the network.

#### Test a specific event

If you want to try your workflow with a specific event you can run the following command:
```
mesg-cli workflow test --event event.json workflow.yml
```

#### Test a live event 

When you want to test that your workflow is working fine with any events you are trying to connect to you can run the following command:

```
mesg-cli workflow test --live workflow.yml
```
