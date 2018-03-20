# Deployment

When you created your [workflow file](https://docs.mesg.tech/workflow/cli/file.html), [validate](https://docs.mesg.tech/workflow/cli/validation.html) it and [test](https://docs.mesg.tech/workflow/cli/testing.md) it you are ready to deploy it.

Each deployment will cost you a small amount of MESG token be aware that this amount is not refundable so please make sure to test your workflow correctly.

You can deploy your workflow using the command

```bash
mesg-cli service deploy
```

If your workflow is valid it will be deployed through the MESG decentralised network and will be ready to be executed

Congratulation you have deploy your first MESG workflow that connect technologies through the MESG decentralised network.



By default, the `test`command looks in the **current folder** for the `mesg.yml` file. If you want to run it in a different folder, you can specify the path to your service folder at the end of any command like so:

```bash
mesg-cli service deploy ./PATH_TO_SERVICE_FOLDER
```



