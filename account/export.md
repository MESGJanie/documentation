# Export accounts

To export your accounts, execute the following method:

```bash
mesg-cli account export
```

**Warning:** This method does **NOT** export your accounts password. You have to manage your password yourself.

This method creates a file containing the information about your accounts. The private keys of your accounts are encrypted with each account's password.

You can import the backup file on any other Application with the [import method](/./account/import.md).

You can also export only one account with the `name` parameter:

```bash
mesg-cli account export --name ACCOUNT_NAME
```



