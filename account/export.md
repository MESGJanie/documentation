# Export accounts

To export your one account, execute the following method:

```bash
mesg-cli account export --account ACCOUNT_NAME
```

**Warning:** This method does **NOT** export your accounts password. You have to manage your password yourself.

This method creates a file containing the information about your account. The private key of your account is encrypted with the account's password.

You can import the backup file on any other Application with the [import method](/./account/import.md).

