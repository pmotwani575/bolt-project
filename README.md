Infrastructure

Prerequisite
To connect with AWS make sure that valid credentials that is we need to configure AWS local profile , for that we need to have
- AWS ACCESS KEY
- AWS SECRET KEY
- PROFILE NAME and Default region


Overview
The following resources are currently used by the project:

an AWS S3 bucket to store build output files

an AWS CloudFront distribution to access the AWS S3 bucket (see octopus-frontend-shared.s3.eu-west-1.amazonaws.com)

a new AWS Public hosted Zone for Storing the DNS Endpoint and also for Registering the ACMs

a new DNS Validated ACM


Currently there are no specific production or staging resources as each product handles their own versions including environment specific configurations. Since all of them are separated by namespaces one instances of the resources can be shared by all products.

CloudFront Distribution + S3 Bucket
# cd ansible
ansible-playbook -i env/inventory octopus-frontend-aws-cloudfront-s3.yml -l shared
