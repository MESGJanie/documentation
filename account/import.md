# Import accounts

To import previously exported accounts file, execute the following method:

```bash
mesg-cli account import ./PATH_TO_BACKUP_FILE
```

This method will import a previously exported backup file of your accounts created with the [export method](/./account/export.md).

You can also import only one account with the `name` parameter:

```bash
mesg-cli account import --name ACCOUNT_NAME ./PATH_TO_BACKUP_FILE
```



