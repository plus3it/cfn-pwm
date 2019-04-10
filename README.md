# cfn-pwm
PWM (https://github.com/pwm-project/pwm) is an open source password self service application for LDAP directories. 
This cloudformation stack creates the infrastructure needed to run PWM behind an application load balancer

# Templates
## Compound Template
The root template that forms the basis of the remaining templates

## ELB Template
The following elements are created:
* Application Load Balancer
  * ALB Listener
  * ALB Target Group
  * Security Group
    * Inbound on TCP/443 from 0.0.0.0/0
    * Outbound on TCP/80 to PWM security group
* Security group for PWM EC2 instance
  * Inbound on TCP/80 from ALB
  * Outbound on all ports to 0.0.0.0/0

## ASG Template
The following elements are created:
* Auto Scaling Group
  * Launch Config
* IAM role for EC2 instance
* IAM instance profile for EC2 instance
* IAM policy to allow S3 access
* IAM policy to allow Cloudwatch agent to record metrics
* IAM policy to allow SSH access to Admin Group
* IAM policy to allows SSH access to other group 

## SNS Template
The following elements are created:
* SNS Topic

## Parameters
* AdminIAMGroup - the IAM group for which to give access to the EC2 instance 
* ALBSslCertificateName - The name (for IAM) or identifier (for ACM) of the SSL certificate to associate with the ELB. The cert must already exist in the service.
* ALBSslCertificateService - The service to use in order to lookup the certificate (IAM or ACM)
* AmiId - AMI ID
* DiscoIAMGroup - an alternate IAM group for which to give access to the EC2 instance (not required)
* InstanceType
* KeyPairName 
* DesiredCapacity
* MinCapacity
* MaxCapacity 
* PrivateSubnetIDs 
* PublicSubnetIDs 
* ConfigBucketName - The S3 bucket to use to grab configuration files 
* GitHubUsername
* GitHubRepo
* SaltPillarFile - The name of the pillar file stored in the S3 config bucket (include the '.sls') 
* SNSSubscriptionEmail - The email address that will be used to subscribe to the SNS topic
* VPC - the VPC for which to deploy the resources