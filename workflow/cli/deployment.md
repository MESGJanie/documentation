# Deployment

When you have finish to create your [workflow file](./file.md), [validate it](./validation.md) and [test it](./testing.md), you are ready to deploy it.

Each deployment will cost you a small amount of {{ book.token }}. Be aware that this amount is **not refundable** so please make sure to test your workflow correctly.

You can deploy your workflow using the command:

```bash
mesg-cli workflow deploy ./worfklow.yml
```

If your workflow is valid, it will be deployed through the {{ book.network }} and will be ready to be executed.

Congratulation! You have deployed your first MESG workflow that connect technologies through the {{ book.network }}.