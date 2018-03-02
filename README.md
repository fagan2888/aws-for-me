# Cheatsheet for getting onto AWS 

This is just for me. YMMV.

# Sources
- https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/AccessingInstancesLinux.html

# Launching
Goto AWS Console. For Cornell, http://signin.aws.cucloud.net/
- when requested, and not previously configured, create a key with PEM file. Download and follow instructions (needed for SSH access)

# Setup
I configured a specific IAM
- create IAM
- 
Download the AWS CLI
  pip install awscli
Run configure command
  aws configure
Fill in 
-  Access Key ID
- 

# Getting the ID of the launched instance `instance-id`
  aws ec2 describe-instances --output table
Possibly
  aws ec2 describe-instances --output table | grep -E "InstanceId|PublicDnsName"

# Connecting to the instance
  ssh -i .ssh/amazon-cornell-default.pem $USER@$PublicDnsName
where the $USER may differ according to the launched AMI (Ubuntu=ubuntu)

