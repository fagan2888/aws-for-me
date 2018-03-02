# Cheatsheet for getting onto AWS 

This is just for me. YMMV.

# Sources
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html
- https://aws.amazon.com/blogs/big-data/running-sparklyr-rstudios-r-interface-to-spark-on-amazon-emr/

# Basics
## Launching
Goto AWS Console. For Cornell, http://signin.aws.cucloud.net/
- when requested, and not previously configured, create a key with PEM file. Download and follow instructions (needed for SSH access)

## Setup
I configured a specific IAM
- create IAM

Download the AWS CLI

    pip install awscli

Run configure command

    aws configure

Fill in 
- Access Key ID
- Secret key
- us-east-2

## Getting the ID of the launched instance `instance-id`
    aws ec2 describe-instances --output table

Possibly

    aws ec2 describe-instances --output table | grep -E "InstanceId|PublicDnsName"

## Connecting to the instance
    ssh -i .ssh/amazon-cornell-default.pem $USER@$PublicDnsName
where the $USER may differ according to the launched AMI (Ubuntu=ubuntu)

## Stopping instances
    aws ec2 stop-instances --instance-ids $InstanceId


# EMR and Spark
from https://aws.amazon.com/blogs/big-data/running-sparklyr-rstudios-r-interface-to-spark-on-amazon-emr/

## Problem: no default InstanceProfile
Solution: https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-iam-roles-defaultroles.html

    aws emr create-default-roles
    
## Problem: unsupported instance type
The example for `sparklyr` uses m3.2xlarge, which no longer exist. Use m4.2xlarge or m4.large.
