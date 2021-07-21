# Introduction

A Stake Pool Operator needs to ensure all relay nodes are performing as expected in their network. There are a variety of tools and health checks that can be done. These are AWS Tools to perform various health check on the Cardano relay nodes.

Check out the sub folders for specific details.

## Canary Topology

Currently, the Cardano network relies on manual peer discovery. To achieve this discovery it is important that you register your nodes using `topologyUpdater.sh`. Once your nodes are registered they will be in the topology master file and available for peer discovery. 

However, if your topology updater script stops working for some reason your nodes will be removed from peer discovery.

To make sure this doesn’t happen we will check the topology file every hour to make sure the nodes are present.

To automate this check we can use AWS Synthetics Canary to check every hour. This is a very cost effective way of making a specific health check.


## External Health

It's important to ensure external connectivity to your relay nodes. 

This creates an external AWS health check on your cardano nodes. It's a simple tcp request to the specified node and port. This isn't as sophisticated as [cncli ping](https://github.com/AndrewWestberg/cncli), which checks for the correct node response. But, the AWS health check is conducted automatically from a geographically dispersed locations. Our assumption is that if we can access the port via tcp, we'll rely on internal `cncli ping` requests to ensure node functionality.

We’ll use a CloudFormation template to create the health check, the alarm, and the SMS notification.



