# Interacting with S3 Buckets via the CLI
## Prerequisites:
An AWS account  

AWS CLI installed on your client.   

## Topics Covered:
Using the AWS console, create a new IAM user with Programmatic administrator access. We will delete this user programmatically at the end of the exercise.  
Using the CLI tool, create a bucket, upload a file to it, and then delete the bucket  
Using the CLI tool, delete an IAM user  

### Step 1. Create a new IAM user
<a href="https://github.com/arabog/IAM-Policy" target="_blank">How to create IAM user</a>

### Step 2. Configure a new profile in the AWS CLI
1. Now, to allow the newly created user to interact with the AWS services via CLI, configure the access key in your CLI tool. Let's create a new profile Babs, and associate the newly created access key to it. You can use the following commands:  

`aws configure --profile Babs`

<a href="https://github.com/arabog/aws-cli" target="_blank">How to configure AWS CLI</a>

You can review the current configuration as:  
Check the configuration
`aws configure list`  

View the content of the credentials and configuration file
`cat ~/.aws/credentials`  
`cat ~/.aws/config`  

Note - You can create multiple profiles, and use a specific one in any aws command using the --profile <profile-name> option.

### Step 3. Interact with S3 service
1. Create a public bucket with name my-20222022-bucket in the us-east-1 region, using the newly created Babs profile.  

`aws s3api  create-bucket --bucket my-20222022-bucket --acl public-read-write --region us-east-1 --profile Babs`  

returns:  
`Location: /my-20222022-bucket`  

In the command above,

`--bucket` option specifies the bucket name of your choice. It must be unique across all AWS accounts.  
`--profile` option specifies the profile whose credentials will be verified for authorization before accessing the S3 service. This option is OPTIONAL. If skipped, the aws CLI will use the default profile stored in your system.  
`--acl` option specifies the accessibility level  
`--region` specifies the AWS region where you want to create this bucket  


2. Upload a file to your bucket. The command below uploads a file names Sample.html  
`aws s3api put-object --bucket my-20222022-bucket --key Sample.html --body Sample.html --profile Babs`  

In the command above,  
`--key` option specifies the name you want to assign to your object in the bucket  
`--body` option specifies the file name (complete path) to upload from your local system  

3. Verify the S3 bucket by going to the AWS web console.  

![bucket](bucket.png?raw=true "bucket")

4. Delete the bucket and its content. A bucket can only be deleted if it is empty. Therefore, first delete the Sample.html, and then delete the bucket, as follows:  

`aws s3api delete-object --bucket my-20222022-bucket --key Sample.html`  
`aws s3api delete-bucket --bucket my-20222022-bucket --profile Babs`  

5. Navigate back to the S3 dashboard (AWS web console), and verify if the bucket has been deleted successfully.  
![bucket2](bucket2.png?raw=true "bucket2")

## Step 4. [Optional] Delete the newly created UdacityLab IAM user
Note - We are assuming that your AWS root user access key is set in the default profile using the command `aws configure --profile default`. Use the `cat ~/.aws/credentials` command to list all the credentials set locally.  

1. View the list of all users (other than the root user) in your AWS account.  
`aws iam list-users`

2. A user can only be deleted only after deleting the attached user policies, and access key. The commands below are executed using the default profile, therefore we have not used the --profile option.

List the policies attached to the Babs user  
`aws iam list-attached-user-policies --user-name Babs`  
A policy can be detached using its ARN, a unique identifier  
`aws iam detach-user-policy --user-name Babs --policy-arn arn:aws:iam::aws:policy/ AdministratorAccess`  
List the access key for Babs user  
`aws iam list-access-keys --user-name Babs`  
An access key can be deleted using its access key id  
`aws iam delete-access-key  --user-name Babs --access-key-id AKIAQGW4TBMDIZQP564S`  
Delete the user  
`aws iam delete-user --user-name Babs`  

3. Navigate back to the IAM console, and access the IAM Users service to verify if the user has been successfully deleted.  

