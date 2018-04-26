---
description: Receiving commands from Core
---

# Listen for Tasks

## Why listen for tasks?

The Service needs to receive the command sent by the Core in order to execute any desired task. Every time a command is received, it will ensure that the sender is the Core, then it will check if it can handle the command, and if so, it will execute it. Once executed, it will reply to the Core with the result of the command.

