# Source

The source a workflow's trigger. Every time the selected event occurs, your workflow will start. This event could come from any of the services running on the MESG infrastructure.

## Properties

### service

> String required

A service is the type of source you want to connect, and will be the ID of the connected service. The service will run on nodes in the network and will emit different kind of events. You can find the list of services running on the MESG platform directly in  or on .

### event

> String required

This is the triggered event from the service that you want to connect. This event will be send from a node on the network and will contain data that you can find on the description of the event in the service detail that you can find in  or on .

### whitelist

> String\[\] optional

The whitelist parameter allows you to accept events from specific nodes in the network. The value for the whitelist will be a list of public node keys. If the whitelist parameter is not set, any source of the event will be accepted. In order to accept any event source, you can also pass the `*` value.

### blacklist

> String\[\] optional

The blacklist parameter allows to reject events from specific nodes in the network. The value for the blacklist will be a list of public node keys. If the blacklist parameter is not set, no nodes will be blacklisted and the event will be accepted from all the network's nodes.

