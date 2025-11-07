# Task 6: Custom VM Image

### Task goals:

- Find some application that can be tested with http protocol (80, no TLS).
- Some possible apps: Apache, Nginx, Lighttpd
- Prepare CLI commands to install application on VM
- Test public Internet access to prepared VM and include screenshot about passing test to task README.txt
- Create VM Image when setup is verified to work.
- Create VM from VM Image
- Test public Internet access to image based VM and include screenshot about passing test to task README.txt

---

### Upload the CloudFormation template "vpc-santeri-vauramo.yaml" to AWS Sandbox CloudShell. (This had to be done in LearnerLab, in sandbox the custom AMI instance could not be launched due to permission error)

### Run the following CLI commands to create the stack and key pair:

```bash
aws cloudformation validate-template --template-body file://vpc-santeri-vauramo.yaml
```
```bash
aws ec2 create-key-pair --key-name learner-vm-key --query 'KeyMaterial' --output text > learner-vm-key.pem
chmod 400 learner-vm-key.pem
aws cloudformation create-stack --stack-name SanteriVPC --template-body file://vpc-santeri-vauramo.yaml --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND --parameters ParameterKey=KeyName,ParameterValue=learner-vm-key ParameterKey=EnvironmentName,ParameterValue=Production
```

### Check the IP of the EC2 instance by searching by Course tag, or check it out from AWS GUI console
```bash
aws ec2 describe-instances \
  --filters "Name=tag:Course,Values=Cloud Architectures - AWS" \
  --query "Reservations[].Instances[].[InstanceId, PublicIpAddress, State.Name]" \
  --output table
```
  
#SSH to the IP, replace the IPv4 address for the one you've got #type "yes"
```bash
ssh -i ./learner-vm-key.pem ec2-user@107.21.68.193
```
### or
```bash
aws ec2-instance-connect ssh --instance-id <instance-id-here>
```
### Update package list, install apache2, enable it on startup, and launch it 
```bash
sudo yum update -y
sudo yum install httpd
sudo systemctl enable httpd
sudo systemctl start httpd
```
### Navigate to EC2 ip "http://107.21.68.193", you will see apache2 test page if all is correct.

![task61](images/task61.png)

### Create Image (AMI) from instance.

### Launch new instance from AMI. Test internet access with instance IP

![task62](images/task62.png)



---

- Based on [Public Cloud Solution Architect](https://pekkakorpi-tassi.fi/courses/pkt-arc/pkt-arc-edu-olt-2025-1e/course.html) course by **Pekka Korpi-Tassi** 2025.
- [Project Prep Tasks](https://pekkakorpi-tassi.fi/courses/pkt-arc/pkt-arc-edu-olt-2025-1e/iac_deployment.html).

Written by **Santeri Vauramo** 2025.
