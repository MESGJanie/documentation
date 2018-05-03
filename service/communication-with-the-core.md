---
description: Sending commands
---

# Emit an Event

## Why emit an Event? 

Events are emitted from a Service \(e.g.: a web server receiving a request, or a blockchain technology receiving a new transaction\). These events are emitted to achieve a desired effect, or to be used as a trigger to make another task happen. Each Service has different kinds of events that you can send to the Core. 

### Steps to follow

In order to define your event you need to :

* [ ] [Add the definition of your event](communication-with-the-core.md#create-your-event) in your [`mesg.yml`](service-file.md) file
* [ ] [Publish your event](communication-with-the-core.md#publish-your-event) when it's happening in your service

## Create your Event

{% tabs %}
{% tab title="Detail" %}
First step to create your event is to update your [`mesg.yml`](service-file.md) file and add an event indexed by it's key with the following attributes :

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- |
| **name** | `id` | `String` | Name of your event, if not set the name will be the same as the id you choose for the event. |
| **description** | `""` | `String` | Describe your event, what's it's purpose and why users might want to connect to it |
| **data** | `{}` | `map<id,`[`Data`](communication-with-the-core.md#data-of-your-event)`>` | The data that your event will contains |

### Data of your event

| **Attribute** | **Default value** | **Type** | **Description** |
| --- | --- | --- | --- | --- |
| **name** | `id` | `String` | Name of your data |
| **description** | `""` | `String` | Description of your data |
| **type** | `String` | [`Type`](communication-with-the-core.md#type-of-your-data) | Type of data |
| **optional** | `false` | `boolean` | Mark you data as optional |

### Type of your data

You can send different types of data. This type can be one of the following :

* `String`
* `Boolean`
* `Number`
* `Object`
{% endtab %}

{% tab title="Example" %}
Here is an example of what your event might looks like in your [`mesg.yml`](service-file.md) file :

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
{% endtab %}
{% endtabs %}

## Publish your event

Your Service needs to publish the event to the core in order to propagate it. In order to do that you have to use the [Protobuffer definition](https://github.com/mesg-foundation/application/blob/dev/types/api_event.go) and [gRPC](https://grpc.io/) to send the data.

{% hint style="info" %}
Consider emitting event when your service is ready. If your service needs to synchronise some data first, you should wait for this synchronisation before emitting the event.
{% endhint %}

### Event.Emit

{% tabs %}
{% tab title="Request" %}
| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- | --- |
| **service** | [Service](service-file.md) | Required | Object containing the service definition loaded from the yml service file. |
| **eventKey** | `String` | Required | The event's key defined in the [service file](https://github.com/mesg-foundation/documentation/tree/c1028b6f9d709adf2ad46364ce7baaa37e27ff8e/service/service/service-file.md) |
| **eventData** | `String` | Required | The event's data in JSON format |

```javascript
{
    "service": {
      ...
      "events": {
        "eventX": {
          "data": {
            "foo": { "type": "String" }
          }
        }
      },
      ...
    },
    "eventKey": "eventX",
    "eventData": "{\"foo\":\"hello\",\"bar\":false}"
}
```
{% endtab %}

{% tab title="Reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- |
| **error** | `String` | A string that contains the error if error present |

```javascript
{
    "error": ""
}
```
{% endtab %}
{% endtabs %}

#### Examples

{% tabs %}
{% tab title="Node" %}
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const grpc = require('grpc')

const yaml = require('js-yaml')
const fs = require('fs')
const api = grpc.load(__dirname + '/api/service/api.proto').types
const eventClient = new apu.Service(
  process.env.MESG_ENDPOINT,
  grpc.credentials.createInsecure()
)

eventClient.EmitEvent({
  service: yaml.safeLoad(fs.readFileSync("./mesg.yml")),
  eventKey: "eventX",
  eventData: JSON.stringify({
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


	api "github.com/mesg-foundation/core/api/service"
	"github.com/mesg-foundation/core/service"
	"google.golang.org/grpc"
	yaml "gopkg.in/yaml.v2"
)

type EventX struct {
	Foo string
	Bar bool
}

func main() {
	content, _ := ioutil.ReadFile("./mesg.yml")
	var service service.Service
	yaml.UnmarshalStrict(content, service)

	connection, _ := grpc.Dial(os.Getenv("MESG_ENDPOINT"), grpc.WithInsecure())
	cli := api.NewServiceClient(connection)

	eventX, _ := json.Marshal(EventX{
		Foo: "hello",
		Bar: false,
	})

	reply, _ := cli.EmitEvent(context.Background(), &api.EmitEventRequest{
		Service:   &service,
		EventKey:  "eventX",
		EventData: string(eventX),
	})
	log.Println(reply)
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



