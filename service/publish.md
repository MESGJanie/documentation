# Publish

When you have finished creating your [service file](service-file.md), have [validated it](validate.md) and [tested it](test.md), you are ready to publish it.

Each publication will cost you a small amount of tokens. Be aware this amount is **not refundable.** So please make sure you tested your service correctly.

To publish a service, run the command:

```bash
mesg-core service publish
```

If you don't set the path to the service folder, the method looks in the current folder for the `mesg.yml` file.

The first time you publish the service, it will be publish as a new service and an unique service ID will be created. This ID will be saved in a `.mesg` file.

When publishing an already-published service \(when a `.mesg` file containing the service ID exists\), the service will be updated on the Network. Users that use this service will have to manually update their workflow to use the last version of the service.

A version ID is created each time you publish the service. This version ID is also saved in the `.mesg` file\).

