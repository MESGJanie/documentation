# Service folder

A service must have its own folder.

The only mandatory file is the `mesg.yml` that contains your Service definitions. See the [Service file](service-file.md) page for more information.

Example of a folder for a Service:

```text
|-- SERVICE_NAME
|   |-- .env
|   |-- .git/
|   |-- .gitignore
|   |-- .mesg
|   |-- .mesgignore
|   |-- mesg.yml
|   |-- Dockerfile
|   |-- source/
|   |   |-- main.go
```

