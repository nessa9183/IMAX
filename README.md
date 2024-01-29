# Cloudformation templates for launching imaxcom stack

### Setup

- Install aws cli and dependencies if not already
- Put access_key_id, secret_access_key and session_token into .aws/credentials

### Example

```
[aws-mkt-dev]
aws_access_key_id=xxxxx
aws_secret_access_key=xxxxx
aws_session_token=xxxxx

```

- Test with:

```
aws --profile=aws-mkt-dev s3 ls
```

- Nested stacks require being in S3. Copy over cloudformation code to S3 bucket in different region.

In aws-mkt-dev account:

- us-west-2: imaxcom-dev-mkt-cloudformation-templates


- us-east-1: imaxcom-dev-mkt-cloudformation-templates-east


- Note on parameters

```
[
  {
      "ParameterKey": "EnvironmentType",
      "ParameterValue": "dev"
  },
  {
       "ParameterKey": "Team",
       "ParameterValue": "Marketing"
  },
  {
       "ParameterKey": "AvailZone1",
       "ParameterValue": "a"
  },
  {
       "ParameterKey": "AvailZone2",
       "ParameterValue": "b"
  },
  {
       "ParameterKey": "Schedule",
       "ParameterValue": "AlwaysRunning"
  }
]
```

- "EnvironmentType" is the environment to be deployed to, allowed values are dev|stage|prod
- "Team" is used for tags to be utilized in Cost explorer. Please tag all any resources that allow tagging in the stack with this.
- "AvailZone1" and "AvailZone2" are defined as parameters to allow flexibility between different regions. Some azs are not necessarily in each region.
- "Schedule" is used for use with potential automatic scheduling for shutdown of resources during non peak hours for non prod environments. EC2 instances and RDS are prime candidates.

### Example launch the imax.com infra23 dev master stack in us-west-2:

```
aws --profile aws-mkt-dev cloudformation create-stack --stack-name imaxcom-dev-mkt --template-body file://imaxcom-master.yaml --parameters file://launch-imaxcom-dev.json --region us-west-2 --capabilities CAPABILITY_NAMED_IAM

```

### Update Master Stack:

```
aws --profile aws-mkt-dev cloudformation create-change-set --stack-name imaxcom-dev-mkt --change-set-name imaxcom-dev-date --template-body file://imaxcom-master.yaml --parameters file://update-imaxcom.json --region us-west-2 --capabilities CAPABILITY_NAMED_IAM

```
### Example launch the imax.com infra23 dev master stack in us-east-1:

```
aws --profile aws-mkt-dev cloudformation create-stack --stack-name imaxcom-dev-mkt --template-body file://imaxcom-master.yaml --parameters file://launch-imaxcom-dev.json --region us-east-1 --capabilities CAPABILITY_NAMED_IAM

```

### Update Master Stack in us-east-1:

```
aws --profile aws-mkt-dev cloudformation create-change-set --stack-name imaxcom-dev-mkt --change-set-name imaxcom-dev-date --template-body file://imaxcom-master.yaml --parameters file://update-imaxcom.json --region us-east-1 --capabilities CAPABILITY_NAMED_IAM

```
Note: All updates to the stack should be from the master stack imaxcom-master.yaml, not at any children in the nested stack.# IMAX
