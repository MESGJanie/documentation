# Listen for events

## Why listen for events?

Applications listen for events in real time to execute actions or tasks when something relevant happens on a technology.

## Listening for events from services

To listen for events, the application needs to open a stream with Core with [gRPC](https://grpc.io/) using the [Protobuffer definition](https://github.com/mesg-foundation/core/blob/dev/api/core/api.proto). When opening the stream, the application listens to the service. It can listen to many service as the same time.

{% tabs %}
{% tab title="Request" %}
### `Client.ListenEvent`

| **Name** | **Type** | **Required** | **Description** |
| --- | --- |
| **service** | `Service` | Required | Object that contains the service definition loaded from the yml [service file](../service/service-file.md). |

```javascript
{
  "service": {
    ...
    "events": {
      "eventX": {
        "data": {
          "dataX": { "type": "String" }
        }
      }
    }
    ...
  }
}
```
{% endtab %}

{% tab title="Stream reply" %}
| **Name** | **Type** | **Description** |
| --- | --- | --- | --- |
| **error** | `String` | A string that contains the error, if an error is present. |
| **eventKey** | `String` | The event's key defined in the [service file](../service/service-file.md). |
| **eventData** | `String` | The event's data in JSON format. |

```javascript
{
  "error": "",
  "eventKey": "eventX",
  "eventData": "{\"dataX\": \"event data\"}"
}
```
{% endtab %}
{% endtabs %}

### Example

{% tabs %}
{% tab title="Node" %}
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const grpc = require('grpc')
const yaml = require('js-yaml')
const fs = require('fs')
const api = grpc.load(__dirname + '/api/core/api.proto').api
const core = new api.Core(
  process.env.MESG_ENDPOINT,
  grpc.credentials.createInsecure()
)

const listenEventStream = core.ListenEvent({
  service: yaml.safeLoad(fs.readFileSync("./serviceX/mesg.yml")),
})
listenEventStream.on('error', function(error) {
  // An error has occurred and the stream has been closed.
})
listenEventStream.on('data', function(data) {
  console.log('receive', data)
})
listenEventStream.on('status', function(status) {
  // process status
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
	"fmt"

	"github.com/mesg-foundation/core/api/core"
	"github.com/mesg-foundation/core/service"
	"google.golang.org/grpc"
)

func main() {
	connection, _ := grpc.Dial(":50052", grpc.WithInsecure())
	cli := core.NewCoreClient(connection)
	stream, _ := cli.ListenEvent(context.Background(), &core.ListenEventRequest{
		Service: &service.Service{},
	})
	for {
		event, _ := stream.Recv()
		fmt.Println(event.EventKey, event.EventData)
		// TODO process event
	}
}

```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}



