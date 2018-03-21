# Publish

When you have finished to create your [service file](/./service/configuration.md), [validate it](/service/validation.md) and [test it](/service/test.md), you are ready to publish it.

Each publication will cost you a small amount of token. Be aware this amount is **not refundable.** So please make sure to test your service correctly.

To publish a service, run the command:

```bash
mesg-cli service publish
```

The first time you publish the service, it will be publish as a new service and an unique service Id will be created. This Id will be saved in a `.mesg` file.

When publishing an already published service \(when a `.mesg` file containing the service Id exist\), the service will be updated on the Network. Users that use this service, will have to manually update their workflow to use the last version of the service.

A version Id is created every time you publish the service. This version id is also saved in the `.mesg` file\).

---

By default, the `publish` command looks in the **current folder** for the `mesg.yml` file. If you want to run it in a different folder, you can specify the path to your service folder at the end of any command like so:

```bash
mesg-cli service publish ./PATH_TO_SERVICE_FOLDER
```



