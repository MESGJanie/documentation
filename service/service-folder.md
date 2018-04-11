# Service folder

A service must have its own folder.

The only mandatory file is the `mesg.yml` that contains all the service definitions. See [service file](https://github.com/mesg-foundation/documentation/tree/b3d92737e4dfd41f30e20d0ab1f2b8dbbf045a2d/service/develop/service-file.md) page.

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

