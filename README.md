## cfn-pwm
PWM (https://github.com/pwm-project/pwm) is an open source password self service application for LDAP directories.
This cloudformation stack creates the infrastructure needed to run PWM behind an application load balancer

## Prequisites
* S3 bucket with the following configuration files
  * `PwmConfiguration.xml` - the config file used to configure PWM
  * `PwmConfiguration.xml.sha1` - file with the SHA1 hash of the pwm configruation file
  * `<pillar_file>.sls` - Name of the pillar file to be used for the salt formula
  * `pwm18.war` - The Java war file used to deploy PWM
  * `pwm18.war.sha1` - file with the SHA1 hash of the pwm war file

## PWM War File
This is a precompile Java WAR file pulled directly from the [PWM project](https://github.com/pwm-project/pwm/)

You can find WAR files [here](https://www.pwm-project.org/artifacts/pwm/)

## Package CloudFormation Template
* Packages the local artifacts (local paths) that your AWS CloudFormation template references.
  * `aws cloudformation package --template-file templates/pwm.compound.json --s3-bucket <value> > packaged_template.json`

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
* PrivateSubnetIDs - list of private subnet ids for the EC2 instances
* PublicSubnetIDs - list of public subnet ids for the application load balancer
* ConfigBucketName - The S3 bucket used to grab configuration files
* GitHubUsername - The github username of the salt formula that is going to be getting deployed
* GitHubRepo - The repository name of the salt formula that is going to be getting deployed
* SaltPillarFile - The name of the pillar file stored in the S3 config bucket (include the '.sls')
* SNSSubscriptionEmail - The email address that will be used to subscribe to the SNS topic
* VPC - the VPC for which to deploy the resources
