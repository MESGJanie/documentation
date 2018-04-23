# Validate

Before deployment, the `mesg.yml` file is verified for syntax verification.

You can also check it manually by using the following command:

```bash
mesg-cli service validate ./PATH_TO_SERVICE_FOLDER
```

If you don't specify the path to the service folder, the method searches in the current folder for the `mesg.yml` file.

All the definitions of the `mesg.yml` file can be found in the [Service File](service-file.md) page.

