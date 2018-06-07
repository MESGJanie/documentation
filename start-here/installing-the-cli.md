# Installing the CLI

## **Download the CLI**

First, download the CLI so you're able to interact with the MESG Core. You can either download the binaries directly from the [release page](https://github.com/mesg-foundation/core/releases/latest) then rename it to `mesg-core` and install it your path, or you can follow the installation process for your system:

{% tabs %}
{% tab title="MacOS" %}
{% code-tabs %}
{% code-tabs-item title="install.sh" %}
```bash
curl https://github.com/mesg-foundation/core/releases/download/release-dev/mesg-core-darwin-10.6-amd64 --progress-bar -L -o ~/.local/bin/mesg-core
chmod +x ~/.local/bin/mesg-core
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="Linux" %}
{% code-tabs %}
{% code-tabs-item title="install.sh" %}
```bash
sudo curl https://github.com/mesg-foundation/core/releases/download/release-dev/mesg-core-linux-amd64 --progress-bar -L -o /usr/local/bin/mesg-core
sudo chmod +x /usr/local/bin/mesg-core
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

Then, to start Core, execute the following in a terminal:

```bash
mesg-core
```

{% hint style="warning" %}
If your system is not listed in the block before, please go to our [GitHub release page](https://github.com/mesg-foundation/core/releases) and download the correct one.
{% endhint %}

### Docker CE

MESG also requires [Docker](https://www.docker.com/) to run on your machine.

If you don't have Docker installed on your machine yet, you can download it now from their official website:

* [Mac OS](https://store.docker.com/editions/community/docker-ce-desktop-mac)
* [Windows](https://store.docker.com/editions/community/docker-ce-desktop-windows)
* [Ubuntu](https://store.docker.com/editions/community/docker-ce-server-ubuntu)
* [Other](https://store.docker.com/search?type=edition&offering=community)

One this is done open a new terminal and type `mesg-core` and you should have something similar to this.

\[\[ TODO: Insert screenshot of the command line \]\]

