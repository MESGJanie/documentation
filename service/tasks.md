# Tasks

## Receiving commands from the Core

The Service needs to receive the command sent by the Core. Every time a command is received, it will make sure that the sender is the Core, then check that it can handle the command, and if so, execute it. Once executed, it will reply to the Core with the result of the command.

