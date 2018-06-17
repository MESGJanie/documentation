# Create your application

Now that you have your different services ready and deployed you need to connect them through an application. You will be able to connect a task to an event with libraries that we provide, otherwise you can always connect directly to the MESG core to [listen for events](listen-for-events.md) and [execute task](execute-a-task.md).

{% tabs %}
{% tab title="Node" %}
{% code-tabs %}
{% code-tabs-item title="index.js" %}
```javascript
const MESG = require('mesg-js').application()

// When SERVICE_EVENT_ID emits event "eventX"
// then execute "taskX" from SERVICE_TASK_ID 
MESG.whenEvent({ serviceID: SERVICE_EVENT_ID, filter: "eventX" }, {
  serviceID: SERVICE_TASK_ID,
  taskKey: 'taskX',
  inputs: (event, data) => ({ foo: 'bar' })
})

// When SERVICE_TASK_ID send the result of taskX
// then execute "taskB" from SERVICE_TASK2_ID
MESG.whenResult({ serviceID: SERVICE_TASK_ID, task: 'taskX' }, {
  serviceID: SERVICE_TASK2_ID,
  taskKey: 'taskB',
  inputs: { hello: "world" }
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
  "os"
  "os/signal"
  "syscall"

  "github.com/mesg-foundation/core/api/client"
)

func main() {
  worfklowA := client.Workflow{
    OnEvent: &client.Event{ServiceID: SERVICE_EVENT_ID, Name: "eventX"},
    Execute: &client.Task{ServiceID: SERVICE_TASK_ID, Name: "taskX",
      Inputs: func(interface{}) interface{} {
        return map[string]string{
          "foo": "bar",
        }
      },
    },
  }

  go worfklowA.Start()
  defer worfklowA.Stop()
  workflowB := &client.Workflow{
    OnResult: &client.Result{ServiceID: SERVICE_TASK_ID, Name: "taskX"},
    Execute: &client.Task{ServiceID: SERVICE_TASK2_ID, Name: "taskB",
      Inputs: func(data interface{}) interface{} {
        return map[string]string{
          "hello": "world",
        }
      },
    },
  }
  go workflowB.Start()
  defer workflowB.Stop()

  abort := make(chan os.Signal, 1)
  signal.Notify(abort, syscall.SIGINT, syscall.SIGTERM)
  <-abort
}
```
{% endcode-tabs-item %}
{% endcode-tabs %}
{% endtab %}
{% endtabs %}

These kind of workflows should be enough for most use cases but if you want to create more complex applications you can connect directly to APIs and keep reading the documentation.

