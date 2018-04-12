# Source

The source is the trigger for your workflow. Every time the selected event occurs, your workflow will start. This event might come from any of the services running on the MESG infrastructure.

## Properties

### service

> String required

The type of the source to connect, this will be the ID of the service to connect. This service will run on some nodes in the network and will emit different kind of events. You can find the list of services running on the MESG platform directly in  or on .

### event

> String required

The event from the service that you want to connect. This event will be send from the node of the network and will contains some data that you can find on the description of the event in the service detail that you can find in  or on .

### whitelist

> String\[\] optional

The whitelist parameter permits to accept events only from specific nodes from the network. The value for the whitelist will be a list of node public keys. If this parameter is not set, any source of the event will be accepted. In order to accept any source of event you can also pass the `*` value.

### blacklist

> String\[\] optional

The blacklist parameter permits to reject events from specific nodes from the network. The value for the blacklist will be a list of node public keys. If this parameter is not set, no nodes will be blacklisted and event will be accepter from all the network's nodes.

