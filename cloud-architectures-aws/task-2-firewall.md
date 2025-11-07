# Task 2: Firewall

### Upload the CloudFormation template "vpc-santeri-vauramo.yaml" to AWS Sandbox CloudShell.

### Run the following CLI commands:

```bash
aws cloudformation validate-template --template-body file://vpc-santeri-vauramo.yaml
aws cloudformation create-stack --stack-name SanteriVPC --template-body file://vpc-santeri-vauramo.yaml --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND --parameters ParameterKey=KeyName,ParameterValue=learner-vm-key ParameterKey=EnvironmentName,ParameterValue=Production
```

### This is a continuation for Task 1: Networking

- Public and Private Network Access Control Lists (NACL) and rules for different ports (columns 344-493 in the .yaml file)
	- Public Subnets: Allow ports 22, 80, 443 from the Internet
	- Private Subnets: Allow ports 22, 80, 443 from Public Subnet
