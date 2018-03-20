# Validation {#validation}

Before deployment, the `mesg.yml` file is verified for syntax verification.

You can also check it manually using the following command:

```
mesg-cli service validate
```

By default, the `validate` command looks in the **current folder** for the `mesg.yml` file. If you want to run it in a different folder, you can specify the path to your service folder at the end of any command like so:

```bash
mesg-cli service validate ./PATH_TO_SERVICE_FOLDER
```



All the definitions of the `mesg.yml` file can be found in the [Service File](/./service/configuration.md) page.

