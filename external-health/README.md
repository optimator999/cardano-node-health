# Cardano Node External Health Check

This creates an external AWS health check on your cardano nodes. It's a simple tcp request to the specified node and port. This isn't as sophisticated as `cncli ping`, which checks for the correct node response. But, the AWS health check is conducted automatically from a geographically dispersed locations. Our assumption is that if we can access the port via tcp, we'll rely on internal `cncli ping` requests to ensure node functionality.

We’ll use a CloudFormation template to create the health check, the alarm, and the SMS notification.

**Step 1**: Sign up for an [AWS account](https://portal.aws.amazon.com/billing/signup?nc2=h_ct&src=header_signup&redirect_url=https%3A%2F%2Faws.amazon.com%2Fregistration-confirmation#/start)

**Step 2**: Setup [aws cli](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-install.html)

**Step 3**: Setup your [security credentials](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)

**Step 4**: Clone the cardano-canary-topology repo


```bash
git clone https://github.com/optimator999/cardano-node-health.git
cd cardano-node-health/external-health
```

**Step 5**: Create the CloudFormation Stack

The default for the Stack is to create an SMS endpoint. If you already have an SMS endpoint, modify the cloudformation template. Otherwise, add your phone number on the command line.

Next, you need to specify the Fully Qualified Domain Name for your relay hosts. Replace `relay1.example.com,relay2.example.com` with your host names. You'll also need to replace `+15555551212` with your phone number.

Run the CloudFormation Command

```
aws cloudformation create-stack --stack-name cardano-canary-topology --template-body file://cardano-canary-topology.yaml --parameters ParameterKey=PhoneNumber,ParameterValue=+15555551212 ParameterKey=Address,ParameterValue=relay1.example.com ParameterKey=Port,ParameterValue=3000
```
