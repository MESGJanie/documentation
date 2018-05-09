# Installing Core

First, [download Core from GitHub](https://github.com/mesg-foundation/core/releases). Be sure to choose the appropriate version, based on your OS.

Once downloaded, you need to add the execution permission and move it to your local bin folder:

{% tabs %}
{% tab title="MacOS" %}
```bash
chmod +x cli-darwin-10.6-amd64  
mv cli-darwin-10.6-amd64 ~/.local/bin/mesg-core
```

To start Core, execute:

```bash
mesg-core
```
{% endtab %}
{% endtabs %}

{% hint style="danger" %}
The following easy installer is not yet available. Please follow the instructions in the previous section to set up Core.
{% endhint %}

Connect your computer with Core with the following command line:

```bash
curl -sSL https://get.mesg.tech | bash
```

Once entered, it will automatically download the [install script](https://get.mesg.tech), execute it on your computer, and take care of all dependencies.

You can also find the beta and previous release on our [GitHub release page.](https://github.com/mesg-foundation/core/releases)

## Docker CE

To provide compatibility with any technology, MESG also requires [Docker](https://www.docker.com/) to run on your machine.

If you don't have Docker installed on your machine yet, you can download it now from their official website:

* [Mac OS](https://www.docker.com/docker-mac)
* [Windows](https://www.docker.com/docker-windows)
* [Other](https://docs.docker.com/engine/installation/)

**Note:**  
If you would rather work from the Docker image, you find more details on [Github](https://github.com/mesg-foundation/application).



