# Publish

when you finish to create your [service file](/./service/configuration.md), [validate it](/service/validation.md) and [test it](/service/test.md), you are ready to publish it.

Each publication will cost you a small amount of token. Be aware this amount is **not refundable.** So please make sure to test your service correctly.

To publish a service, run the command:

```bash
mesg-cli service publish
```

If your service is valid, it will be published on the Network.

By default, the `publish` command looks in the **current folder** for the `mesg.yml` file. If you want to run it in a different folder, you can specify the path to your service folder at the end of any command like so:

```bash
mesg-cli service publish ./PATH_TO_SERVICE_FOLDER
```



