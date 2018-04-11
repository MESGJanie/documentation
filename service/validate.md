# Validate

Before deployment, the `mesg.yml` file is verified for syntax verification.

You can also check it manually using the following command:

```bash
mesg-cli service validate ./PATH_TO_SERVICE_FOLDER
```

If you don't specify the path to the service folder, the method looks in the current folder for the `mesg.yml` file.

All the definitions of the `mesg.yml` file can be found in the [Service File](https://github.com/mesg-foundation/documentation/tree/b3d92737e4dfd41f30e20d0ab1f2b8dbbf045a2d/service/configuration.md) page.

