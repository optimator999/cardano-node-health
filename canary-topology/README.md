# Cardano Topology Canary

Currently, the Cardano network relies on manual peer discovery. To achieve this discovery it is important that you register your nodes using `topologyUpdater.sh`. Once your nodes are registered they will be in the topology master file and available for peer discovery. 

However, if your topology updater script stops working for some reason your nodes will be removed from peer discovery.

To make sure this doesn’t happen we will check the topology file every hour to make sure the nodes are present.

To automate this check we can use AWS Synthetics Canary to check every hour. This is a very cost effective way of making a specific health check.

We’ll use a CloudFormation template to create the canary, the alarm, and the SMS notification.

**Step 1**: Sign up for an [AWS account](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)

**Step 2**: Setup [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

**Step 3**: Setup your [security credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

**Step 4**: Clone the cardano-canary-topology repo


```bash
git clone https://github.com/optimator999/cardano-node-health.git
cd cardano-node-health/canary-topology
```

**Step 5**: Create the CloudFormation Stack

The default for the Stack is to create an SMS endpoint. If you already have an SMS endpoint, modify the cloudformation template. Otherwise, add your phone number on the command line.

Next, you need to specify the Fully Qualified Domain Name for your relay hosts. Replace `relay1.example.com,relay2.example.com` with your host names.

Run the CloudFormation Command

**NOTE**: Once started, the canary will alarm. It will take 3 hours for the carnary to get enough data points to turn off the alarm.

```
aws cloudformation create-stack  --capabilities CAPABILITY_NAMED_IAM --stack-name cardano-canary-topology --template-body file://cardano-canary-topology.yaml --parameters ParameterKey=PhoneNumber,ParameterValue=+15555551212 ParameterKey=RelayNodes,ParameterValue=relay1.example.com\\,relay2.example.com
```

