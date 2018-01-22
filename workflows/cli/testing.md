# Testing

Before deploying your worflow you might want to test it to ensure that everything is working as expected.

Every time you run some test all the services that you need to execute will be executed on your computer and this service will also be available for the rest of {{ book.network }} so your computer might process some other commands.

> We recommand to test with a live event in order to have some valid data and then use the previous results from the live event to retry with the custom event if you need to debug your workflow.

## Testing with a live event

Using the `livetest` command, the source's service will run aditionnaly to the command's services so this might take some ressource from your computed. Also this source's service will be participating for the {{ book.network }}.

```bash
mesg-cli livetest workflow.yml
```

Using the `livetest` command, all the event processed that matches your workflow will be dumped as file in order to let you reuse them with a [custom event](#testing-with-a-custom-event)

##### Warning
The live event cannot be use to process your workflow, the live event testing might stop after a certain time of inactivity and the events receive will not be validated by the MESG concensus so the event might be invalid.

## Testing with a custom event

With a `test` command, the source's service will not run, and the test will assume that the json file is valid and have the data that the source's service might emit.

```bash
mesg-cli test workflow.yml eventfile.json
```