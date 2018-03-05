# What is a Node ?

A Node is an application running and registered on the MESG Network. Nodes are important because MESG is a decentralized infrastructure and the processing is done by the different nodes of the network.



By running the {{ book.wallet }} you will be a node of the MESG Network but still not really participating to the Network. Participating to the network might use some resources on your computer so this should be an action that only you can take.



Every time you register as a node for a specific service you might be selected for one of the following features :

* **Submit an event to the MESG Network**: This is useful in order to start workflows based on some external events
* **Process a task**: Every time a task of workflow is triggered you might be selected to process the task
* **Validate a task**: Every time a node process a task, other nodes will need to validate the result \(if possible\) and your node can be selected as a validator



As you can see your node will have a big responsibility in the MESG Network, in order to secure this you will need to provide a **stake** and a **period of time** that you are willing to run your node.

#### Stake

The stake is a system that "punish" bad actors in the Network. Every Node of the Network needs to deposit a certain amount of {{ book.token }} to show their sincerity. If they behave correctly they will be able to withdraw their deposit and recover 100% of it. If they misbehave, for every bad behavior they had, a penalty will be applied and a part of their stake will be lost and if there is too many bad behaviors the full stake might be taken.

#### Period of time

In order to have the most stable network and know which Node is reliable, you need to set a period of time that you will run the service. If the period of time is not respected this will lead to a penalty based on the stake as described in the [stake part](#stake)



The stake and the period of time will be important because the selection of the nodes is based on those parameters. It is then recommended to have a high stake and a high period of time. Of course you can anytime create a node with no stake and no guarantee of time but the probability of being selected will be low.

