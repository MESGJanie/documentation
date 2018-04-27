---
description: Sending commands
---

# Emit an Event

## Why emit an Event? 

Events are emitted from a Service \(e.g.: a web server receiving a request, or a blockchain technology receiving a new transaction\). These events are emitted to achieve a desired effect, or to be used as a trigger to make another task happen. Each Service has different kinds of events that you can send to the Core. 

## Create your Event

First step to create your event is to update your [`mesg.yml`](service-file.md) file and add an event indexed by it's key with the following attributes :

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- |
| **name** | `id` | `String` | Name of your event, if not set the name will be the same as the id you choose for the event. |
| **description** | `""` | `String` | Describe your event, what's it's purpose and why users might want to connect to it |
| **data** | `{}` | `map<id,`[`Data`](communication-with-the-core.md#data-of-your-event)`>` | The data that your event will contains |

### Data of your event

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| name | `id` | `String` | Name of your data |
| description | `""` | `String` | Description of your data |
| type | `String` | [`Type`](communication-with-the-core.md#type-of-your-data) | Type of data |
| optional | `false` | `boolean` | Mark you data as optional |

### Type of your data

You can send different types of data. This type can be one of the following :

* `String`
* `Boolean`
* `Number`
* `Object`

Here is an exemple of what your event might looks like in your [`mesg.yml`](service-file.md) file :

{% code-tabs %}
{% code-tabs-item title="mesg.yml" %}
```yaml
...

events:
    eventX:
        name: "Event X"
        description: "This is event X"
        data:
            foo:
                name: "Foo"
                description: "Foo is the first data"
                type: String
                optional: false
            bar:
                name: "Bar"
                description: "Bar is the second data"
                type: Boolean
                optional: true
...
```
{% endcode-tabs-item %}
{% endcode-tabs %}

## Publish your event

You service needs to publish the event to the core in order to propagate it. In order to do that you have to use the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go) and [gRPC](https://grpc.io/) to send the data.

### Event.Emit

{% tabs %}
{% tab title="Request" %}
| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| Service | [Service](service-file.md) | Required | Object containing the service definition loaded from the yml service file. |
| Event | `String` | Required | The event's id defined in the [service file](https://github.com/mesg-foundation/documentation/tree/c1028b6f9d709adf2ad46364ce7baaa37e27ff8e/service/service/service-file.md) |
| Data | `String` | Required | The event's data in JSON format |

```javascript
{
    "Service": {
      ...
      "events": {
        "eventX": {
          "data": {
            "foo": { "type": "string" }
          }
        }
      },
      ...
    },
    "Event": "eventX",
    "Data": "{\"foo\":\"bar\"}"
}
```
{% endtab %}

{% tab title="Reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- |
| Error | `String` |  |
| Event | `String` |  |
| Data | `String` |  |

```javascript
{
    "error": "",
    "event": "ethereum_newBlock",
    "data": "{\"number\":2323232}"
}
```
{% endtab %}
{% endtabs %}

#### Exemples

{% tabs %}
{% tab title="Node" %}
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const grpc = require('grpc')

const yaml = require('js-yaml')
const fs = require('fs')
const types = grpc.load(__dirname + '/types/api_event.proto').types
const eventClient = new types.Event(
  process.env.MESG_ENDPOINT,
  grpc.credentials.createInsecure()
)

eventClient.Emit({
  service: yaml.safeLoad(fs.readFileSync("./mesg.yml")),
  event: "eventX",
  data: JSON.stringify({
    foo: "hello",
    bar: false
  })
}, (err, reply) => {
  // handle response if needed
})
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}

{% tab title="Go" %}
{% code-tabs %}
{% code-tabs-item title="main.go" %}
```go
package main

import (
	"context"
	"encoding/json"
	"io/ioutil"
	"log"
	"os"


	"github.com/mesg-foundation/core/types"
	"google.golang.org/grpc"
	yaml "gopkg.in/yaml.v2"
)

type EventX struct {
	Foo string
	Bar bool
}

func main() {
	content, _ := ioutil.ReadFile("./mesg.yml")
	var service types.ProtoService
	yaml.UnmarshalStrict(content, service)

	connection, _ := grpc.Dial(os.Getenv("MESG_ENDPOINT"), grpc.WithInsecure())
	cli := types.NewEventClient(connection)

	eventX, _ := json.Marshal(EventX{
		Foo: "hello",
		Bar: false,
	})

	reply, _ := cli.Emit(context.Background(), &types.EmitEventRequest{
		Service: &service,
		Event:   "eventX",
		Data:    string(eventX),
	})
	log.Println(reply)
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

{% page-ref page="tasks.md" %}



