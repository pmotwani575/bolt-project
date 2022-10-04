# Infrastructure

# Prerequisite
To connect with AWS make sure that valid credentials that is we need to configure AWS local profile , for that we need to have
- AWS ACCESS KEY
- AWS SECRET KEY
- PROFILE NAME and Default region ( for my example I have created on my account and profile name is ITP )


# Overview
The following resources are currently used by the project:

an AWS S3 bucket to store build output files

an AWS CloudFront distribution to access the AWS S3 bucket

a new AWS Public hosted Zone for Storing the DNS Endpoint and also for Registering the ACMs

a new DNS Validated ACM

# How to document - Steps to create setup on a new AWS Account 

Steps need to do on AWS Account 

- Create a new AWS Account 
- Login with Root user , create IAM SAML Roles  if OIDC or SAML Provider is there otherwise just create a IAM user with Administrator policy
- Need to create a local IAM User/profile locally/CLI with ( Secret key and access key ) from above step
- Create a new AWS Route 53 Hosted Zone., for that need to register the domain name in Route 53 , for eg if we want to create bolt.com zone , we need to register the domain first ( In my case I have create a sub hosted zone under parent zone ) , by name blt.searchmetrics.com 
- If we are creating a new sub zone under parent zone like myself , we need to register its NS records in Parent zone 
- Once the Zone ( either parent or child ) is created, we need to create Public ACM certificate in us-east-1 ( Virginia) Region as a mandatory requirement for Cloud front . ( ACM certificate should be in virginia region if we need to setup https connection between viewers and cloudfront)

Steps Need to do on CLI with Ansible Playbook 

- we have create ansible playbook for creating a Cloudfront distribtion , AWS S3 and route 53 CNAME record 
- Attached Ansible Playbook in the github Repositry does the following 

1) Create Bucket and Bucket Policy adding CLoud front Origin Identity as trust
2) CFN Distribution , Cache policy 
3) Route 53 Alias Record 

How to run 

1) Folt the entire bolt-project repositry or clone 
2) use the same structure and modify the aws_all.yml global vairbales as per your environment , ( Bukcet name , region etc ) 
3) In the template , aws_bolt_cloudfront_s3.yml.j2 modify ACM certificate from the above steps ( done on AWS Account )

Once above steps are done , need to run the Ansible playbook , needless to mention need to install Ansible first 

CloudFront Distribution + S3 Bucket
# Final Ansible Command

ansible-playbook -i env/inventory bolt-aws-cloudfront-s3.yml -l shared

# Assignment Feedback

- Assignment is done in a single account only , requirement was to create S3 and Cloudfront in separate account 
- I focussed to work on the solution initially and it is working perfectly fine for me in single account 
- Tried to look for the ways where we can separate S3 ( in one account ) and distribution in another account as per the requirement but in the favor of time thought to submit like this ( Ideally solved the problem with the working solution so did not wanted to invest time on separating the workflows on separate accounts )

It can be possible however with some more investigation below -
- We can create separate playbooks or lets say separate Terraform configuration for separate account provisioning S3 and Cloud front in separate account
 
- Since Cloud formation do not work with Cross account Exports , we can create a dummy S3 parameter in the master account ( where the cloud front is) and using that dummy parameter we can import S3 in the ansible template ( which is submitted ) 



