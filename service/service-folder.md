# Service folder

A service must have its own folder.

The only mandatory file is the `mesg.yml` that contains all the service definitions. See [service file](service-file.md) page.

Example of a folder of a service:

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

