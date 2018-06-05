# Listen for events

## Why listen for events?

Applications listen for events in real time to execute actions or tasks when something relevant happens on a technology.

## Listening for events from services

To listen for events, the application needs to open a stream with Core with [gRPC](https://grpc.io/) using the [Protobuffer definition](https://github.com/mesg-foundation/core/blob/dev/api/core/api.proto). When opening the stream, the application listens to the service. It can listen to many service as the same time.

{% tabs %}
{% tab title="Request" %}
### `Client.ListenEvent`

| **Name** | **Type** | **Required** | **Description** |
| --- | --- | --- |
| **serviceID** | `String` | Required | ID of the service that you want to listen. |
| **eventFilter** | `String` | Optional | Only listens for this given event ID. |

```javascript
{
  "serviceID": "v1_fe25be776e1e256400c77067a1cb7666",
  "eventFilter": "eventIDToOnlyListenTo"
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
const api = grpc.load(__dirname + '/api/core/api.proto').api
const core = new api.Core(
  "localhost:50052",
  grpc.credentials.createInsecure()
)

const listenEventStream = core.ListenEvent({
  serviceId: "v1_fe25be776e1e256400c77067a1cb7666",
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
		ServiceID: "v1_fe25be776e1e256400c77067a1cb7666",
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



