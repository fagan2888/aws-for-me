# Cheatsheet for getting onto AWS 

This is just for me. YMMV.

# Sources
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html
- https://aws.amazon.com/blogs/big-data/running-sparklyr-rstudios-r-interface-to-spark-on-amazon-emr/
- http://bit.ly/cu-aws-2018-08-22 (tutorial by Paul Allen on that date)

## Cornell-specific
- https://confluence.cornell.edu/display/CLOUD/Standard+Tagging

# Basics
## Launching
Goto AWS Console. For Cornell, http://signin.aws.cucloud.net/
- Search for EC2 Console
- Goto "Key Pairs"
- when requested, and not previously configured, create a key with PEM file. Download and follow instructions (needed for SSH access)
- download PEM file (put it into a known location)

## Setup
I configured a specific IAM
- create IAM

Download the AWS CLI

    pip install awscli

or (openSUSE >= 15)
    
    zypper install aws-cli

Run configure command

    aws configure

Fill in 
- Access Key ID
- Secret key
- us-east-2

## Launch an instance

Note: possibility to get billing alerts

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

## Problem: how to stop the cluster
The aws cli spits out an id:

    j-26WVR4U3W5ID7

Stopping the cluster then is

    aws emr terminate-clusters --cluster-ids j-26WVR4U3W5ID7
## Finding out stuff about the cluster
Assuming you didn't terminate it,

    aws emr describe-cluster --cluster-id j-289A9I8T2GVNJ --output table

## Fails to start because of absence of VPC
As configured, the instance doesn't actually start:
   
     Subnet is required : The specified instance type m4.large can only be used in a VPC.

Solution: Go to 

## Problem: The given SSH key name was invalid
The key name is not the access key ID (in the IAM console [https://console.aws.amazon.com/iam/home]), but rather the name you gave the key in the EC2 console [https://console.aws.amazon.com/ec2/home#c=EC2&s=KeyPairs] 

## Logging into the EMR cluster head node
The instructions say "Once the cluster is launched, set up the SSH tunnel[https://docs.aws.amazon.com/emr/latest/ManagementGuide/emr-ssh-tunnel.html] ..." which requires you to know the Public DNS name of the cluster.

# After ALL that

    Code   |  BOOTSTRAP_FAILURE                                                                                     ||||
    Message|  Master instance (i-0e11136ca0da02815) failed attempting to download bootstrap action 1 file from S3   ||||
