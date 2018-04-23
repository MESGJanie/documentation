# Communication with the Core

## Send events to the Core

The Service can send data to the Application. The data is divided into three categories:

* Result of a command: When the Application asks to start a command and expects a result from this command
* Events from listener: When the Service emits a new event from its listener function. \(Eg: web server running and receiving a request or a blockchain technology that received a new transaction\)
* System events: When the service is ready to be executed, notify the Application

```java
service Event {
    rpc Emit (EmitEventRequest) returns (EventReply) {}
}
message EmitEventRequest {
    ProtoService service = 1; 
    string event = 2;
    string data = 3;
}
message EventReply {
    string event = 1;
    string data = 2;
}


```

{% api-method method="post" host="service." path="Emit" %}
{% api-method-summary %}
service.Emit
{% endapi-method-summary %}

{% api-method-description %}

{% endapi-method-description %}

{% api-method-spec %}
{% api-method-request %}
{% api-method-body-parameters %}
{% api-method-parameter name="dwdwdw" type="string" required=false %}
wdwdwd
{% endapi-method-parameter %}
{% endapi-method-body-parameters %}
{% endapi-method-request %}

{% api-method-response %}
{% api-method-response-example httpCode=200 %}
{% api-method-response-example-description %}

{% endapi-method-response-example-description %}

```
string data = 2;
```
{% endapi-method-response-example %}
{% endapi-method-response %}
{% endapi-method-spec %}
{% endapi-method %}

## Receiving commands from the Core

The Service needs to receive the command sent by the Application. Every time a command is received, it will make sure that the sender is the Application, then check that it can handle the command, and if so, execute it. Once executed, it will emit an event to the Application with the result of the command.

**TODO: a technical definition**

