# Task 4 Object Storage

### Upload the CloudFormation template "vpc-santeri-vauramo.yaml" and "tfile.txt" to AWS Sandbox CloudShell.

### Run the following CLI commands to validate and create the stack:
```bash
aws cloudformation validate-template --template-body file://vpc-santeri-vauramo.yaml
```
```bash
aws cloudformation create-stack --stack-name SanteriVPC --template-body file://vpc-santeri-vauramo.yaml --capabilities CAPABILITY_NAMED_IAM CAPABILITY_AUTO_EXPAND --parameters ParameterKey=EnvironmentName,ParameterValue=Production
```
 
### Check the names of the buckets (bucket names are randomized "protected-bucket-xxxxxxxxx-task4") to tackle conflicting names

```bash
aws s3 ls
```
### Copy uploaded tfile.txt with api method:
```bash
aws s3api put-object \
    --bucket BUCKET_NAME_HERE \
    --key tfile.txt \
    --body tfile.txt
```
### Another, easier method
```bash
aws s3 cp tfile.txt s3://BUCKET_NAME_HERE/my-dir/
```
### Upload to protected bucket using the access point ARN
```bash
aws s3 cp tfile.txt s3://arn:aws:s3:us-east-1:ACCOUNT-ID:accesspoint/S3BucketProtectedAccessPoint/
```
### Check if the files are actually in the buckets!
```bash
aws s3 ls s3://BUCKET_NAME_HERE/ --recursive
```




### In my test, the bucket names became:

protected-bucket-471112899714-task4

public-bucket-471112899714-task4

# the commands:
```bash
aws s3api put-object \
    --bucket public-bucket-471112899714-task4 \
    --key tfile.txt \
    --body tfile.txt
aws s3api put-object \
    --bucket protected-bucket-471112899714-task4 \
    --key tfile.txt \
    --body tfile.txt
aws s3 ls s3://protected-bucket-471112899714-task4/ --recursive
aws s3 ls s3://public-bucket-471112899714-task4/ --recursive
```

![readmetask4](images/readmetask4.png)
