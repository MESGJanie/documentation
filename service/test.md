# Test {#testing}

Before deploying your service you might want to test it to ensure that everything is working as expected.

The `test `command have different options but always requires the path to the service file you want to test.

## Test an event

If you want to try your workflow with a specific event you can run the following command:+

```
mesg-cli service test --event event.json service.yml
```

## Test a task

When you want to test that your workflow is working fine with any events you are trying to connect to you can run the following command:

```
mesg-cli service test --live workflow.yml
```

Test a 

