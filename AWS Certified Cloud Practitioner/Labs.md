**AWS Certified Cloud Practitioner Practical Labs (CLF-C02)**


---


### **Lab 1: Explore AWS Global Infrastructure**

**Objective**: Learn about AWS Regions, Availability Zones (AZs), and Edge Locations.

**Steps**:

1. Open the AWS Global Infrastructure page ([https://aws.amazon.com/about-aws/global-infrastructure/](https://aws.amazon.com/about-aws/global-infrastructure/)).
2. Use the interactive map to view available AWS regions and AZs.
3. Note the services available in each region.
4. In the AWS Console, select different regions and view their available services.

**Expected Outcome**: Understand the structure and geographical distribution of AWS infrastructure.

### **Lab 2: Create an S3 Bucket and Host a Static Website**

**Objective**: Host a static website using Amazon S3.

**Steps**:

1. Go to the S3 service in the AWS Console.
2. Create a new bucket with a globally unique name.
3. Enable "Static Website Hosting" in bucket properties.
4. Upload an index.html and error.html.
5. Set permissions to allow public read access.
6. Test the website using the provided S3 endpoint URL.

**Expected Outcome**: Static website should be accessible via the internet.

### **Lab 3: Launch an EC2 Instance**

**Objective**: Deploy a virtual server using EC2.

**Steps**:

1. Open EC2 in AWS Console.
2. Click "Launch Instance".
3. Choose Amazon Linux 2 AMI (free tier eligible).
4. Select t2.micro instance type.
5. Create or select a key pair.
6. Configure a security group to allow SSH (port 22).
7. Launch the instance and connect using EC2 Instance Connect or SSH.

**Expected Outcome**: A running virtual server accessible via terminal.

### **Lab 4: Create IAM User and Group with Policy**

**Objective**: Implement least privilege access using IAM.

**Steps**:

1. Open IAM service.
2. Create a new IAM group and attach the AmazonS3ReadOnlyAccess policy.
3. Create a new IAM user and assign it to the group.
4. Generate console sign-in credentials.
5. Log in as the new user to verify access.

**Expected Outcome**: IAM user can only read from S3, not write or modify.

### **Lab 5: Enable MFA for Root and IAM User**

**Objective**: Enhance security by setting up multi-factor authentication.

**Steps**:

1. Login with root account and go to IAM -> Users.
2. Click on Security credentials tab.
3. Under MFA, choose "Activate MFA".
4. Choose "Virtual MFA device" and use an app like Google Authenticator.
5. Scan the QR code and enter two consecutive OTP codes to verify.
6. Repeat steps for an IAM user.

**Expected Outcome**: MFA is enabled, adding a second layer of protection.

### **Lab 6: Create and Query DynamoDB Table**

**Objective**: Learn how to create a NoSQL database and perform basic operations.

**Steps**:

1. Navigate to DynamoDB in the AWS Console.
2. Create a new table with a partition key (e.g., 'ID').
3. Use default settings to stay within Free Tier.
4. Add items (data entries) using the "Explore Table" tab.
5. Query the table using the built-in query editor.

**Expected Outcome**: Understand how to use DynamoDB to store and retrieve data.

### **Lab 7: Create and Attach IAM Role to EC2**

**Objective**: Use IAM roles to allow EC2 instances to access AWS services securely.

**Steps**:

1. Create a new IAM role for EC2.
2. Attach the AmazonS3ReadOnlyAccess policy.
3. Launch an EC2 instance and attach the IAM role.
4. SSH into the EC2 instance and run AWS CLI commands like:
   `aws s3 ls`
5. Verify access to S3 buckets without using credentials.

**Expected Outcome**: EC2 instance can access S3 securely using role-based permissions.

### **Lab 8: Create Custom VPC with Subnets**

**Objective**: Understand basic networking by creating a Virtual Private Cloud.

**Steps**:

1. Go to VPC Dashboard and create a new VPC.
2. Add one public and one private subnet.
3. Create and attach an Internet Gateway to the VPC.
4. Modify route tables to enable public subnet access to the internet.
5. Launch EC2 instances in both subnets to test accessibility.

**Expected Outcome**: You should be able to access the public subnet EC2 and observe that the private one is isolated.

### **Lab 9: Setup CloudWatch Alarms on EC2 Metrics**

**Objective**: Monitor resource metrics using CloudWatch.

**Steps**:

1. Go to CloudWatch -> Alarms -> Create Alarm.
2. Select the EC2 instance metric (CPUUtilization).
3. Set a threshold (e.g., 70% CPU for 5 minutes).
4. Create or select an SNS topic for notifications.
5. Subscribe your email to the SNS topic.
6. Simulate high CPU usage and observe alert notification.

**Expected Outcome**: You receive an email alert when the CPU threshold is breached.

### **Lab 10: Use AWS Pricing Calculator**

**Objective**: Estimate costs for various AWS services.

**Steps**:

1. Visit the AWS Pricing Calculator website.
2. Add services like EC2, S3, and RDS to the estimate.
3. Specify configurations (instance type, storage size, usage time).
4. View and analyze the monthly estimate.
5. Export or save the estimate for documentation.

**Expected Outcome**: Gain understanding of pricing structure for common services.

### **Lab 11: Create a Budget with Notifications**

**Objective**: Learn how to monitor and control cloud expenses.

**Steps**:

1. Navigate to AWS Budgets in the Billing dashboard.
2. Create a new budget (cost budget).
3. Set your threshold (e.g., \$10/month).
4. Create an alert for 80% usage.
5. Subscribe an email for notification delivery.
6. Review the budget status periodically.

**Expected Outcome**: Email alerts help prevent unexpected charges and overages.

### **Lab 12: Create EBS Volume and Attach to EC2**

**Objective**: Understand how to use EBS volumes for additional storage.

**Steps**:

1. Go to EC2 -> Volumes and create a new EBS volume in the same AZ as your EC2.
2. Attach the volume to a running EC2 instance.
3. SSH into the instance.
4. Use `lsblk` to identify the new device (e.g., /dev/xvdf).
5. Format the volume using `sudo mkfs -t ext4 /dev/xvdf`.
6. Mount the volume with `sudo mount /dev/xvdf /mnt`.

**Expected Outcome**: EC2 instance has additional mounted storage.

### **Lab 13: Create Amazon EFS and Mount on EC2**

**Objective**: Set up Amazon EFS and share files between EC2 instances.

**Steps**:

1. Open EFS service and create a new file system.
2. Attach EFS to a VPC with at least two subnets.
3. Launch two EC2 instances in those subnets.
4. Install NFS client: `sudo yum install -y amazon-efs-utils`
5. Mount EFS: `sudo mount -t efs fs-xxxx:/ /mnt/efs`
6. Create a file on one EC2 and verify visibility on the other.

**Expected Outcome**: Shared storage between multiple EC2s using EFS.

### **Lab 14: Create CloudFront Distribution**

**Objective**: Use CloudFront to serve an S3 static website securely and globally.

**Steps**:

1. Create an S3 bucket and enable static website hosting.
2. Upload HTML files to the bucket.
3. Go to CloudFront and create a distribution.
4. Set the S3 bucket as the origin.
5. Enable caching, logging, and default root object.
6. Access your site using the CloudFront URL.

**Expected Outcome**: Faster global delivery of your S3-hosted content.

### **Lab 15: Create SNS Topic and Send Notification**

**Objective**: Use SNS to broadcast messages to multiple endpoints.

**Steps**:

1. Go to SNS and create a new topic.
2. Choose Standard topic type and name it.
3. Create a subscription using your email address.
4. Confirm subscription via email.
5. Publish a message to the topic.

**Expected Outcome**: Receive the published message at your email.

### **Lab 16: Create and Use AWS CloudTrail**

**Objective**: Track user activity and API calls across AWS.

**Steps**:

1. Go to CloudTrail in the AWS Console.
2. Create a new trail and enable logging to an S3 bucket.
3. Perform actions in your account (e.g., create/delete an EC2).
4. Access the S3 bucket and download the log file.
5. Review entries to see what actions were logged.

**Expected Outcome**: Understand auditing and security monitoring via CloudTrail logs.

### **Lab 17: Launch Elastic Beanstalk Environment**

**Objective**: Deploy a web application quickly using AWS Elastic Beanstalk.

**Steps**:

1. Create a simple Node.js or Python app zip file with app.js and package.json.
2. Go to Elastic Beanstalk -> Create new environment -> Web server environment.
3. Choose platform (e.g., Node.js), upload your zip file.
4. Deploy and monitor environment health.
5. Access the deployed app using the Beanstalk URL.

**Expected Outcome**: Your app is live and managed via Elastic Beanstalk.

### **Lab 18: Use AWS Trusted Advisor**

**Objective**: Use AWS Trusted Advisor to review and analyze your AWS account for cost optimization, security, fault tolerance, performance, and service limits.

**Steps**:

1. Navigate to the AWS Management Console.
2. Go to the Trusted Advisor service from the Services menu.
3. View the dashboard — under "Core Checks" available in the Free Tier.
4. Review checks under the following categories:

   * **Cost Optimization**: Unused or underutilized resources (e.g., idle EC2 instances, unattached EBS volumes)
   * **Security**: IAM use, MFA on root, security group rules
   * **Fault Tolerance**: S3 versioning, snapshot configuration
   * **Performance** and **Service Limits**: High usage approaching service limits
5. Take notes on any warnings or recommendations shown.
6. Apply one or two suggested optimizations manually, if feasible (e.g., delete idle resources or set IAM policy).

**Expected Outcome**: You will be familiar with the checks offered by Trusted Advisor and learn how to interpret AWS’s recommendations to improve cost, security, and performance.

### **Lab 19: Create AWS Support Case (Basic Plan)**

**Objective**: Learn how to interact with AWS Support.

**Steps**:

1. Go to AWS Console -> Support Center.
2. Click "Create case" and select "Account and billing support".
3. Fill in details for a general inquiry or billing issue.
4. Choose contact method (email for Basic plan).
5. Submit the case.

**Expected Outcome**: Understand the support ticket process under the Basic Support plan.

### **Lab 20: Configure Security Groups and NACLs**

**Objective**: Learn how to secure AWS resources using network control mechanisms.

**Steps**:

1. Go to the EC2 dashboard and locate your running instance.
2. Modify its Security Group to allow inbound HTTP (port 80) and SSH (port 22).
3. Test access by opening a web server (e.g., Apache) or connecting via SSH.
4. Go to VPC -> Network ACLs and edit inbound/outbound rules.
5. Block a specific IP address for testing.

**Expected Outcome**: Gain experience controlling network access to instances using SGs and NACLs.

### **Lab 21: Enable and Analyze AWS Config**

**Objective**: Track resource configuration history and compliance.

**Steps**:

1. Go to AWS Config in the Console.
2. Click "Get started" and enable AWS Config.
3. Select the resources to track (e.g., EC2, IAM, S3).
4. Create a rule like "s3-bucket-public-read-prohibited".
5. Analyze the rule compliance status.

**Expected Outcome**: Understand AWS Config’s role in governance and compliance.

### **Lab 22: Schedule AWS Lambda with EventBridge**

**Objective**: Trigger a Lambda function automatically using a schedule.

**Steps**:

1. Create a basic Lambda function (e.g., logs current time).
2. Go to EventBridge -> Create rule.
3. Set a fixed interval schedule (e.g., every 5 minutes).
4. Choose Lambda as the target.
5. Monitor logs in CloudWatch.

**Expected Outcome**: Learn serverless scheduling using EventBridge.

### **Lab 23: Use AWS S3 Lifecycle Rules**

**Objective**: Automatically transition or delete objects in a bucket.

**Steps**:

1. Upload multiple test files into an S3 bucket.
2. Go to Management -> Lifecycle rules.
3. Create a rule to transition objects to Glacier after 1 day.
4. Add another rule to delete them after 7 days.
5. Revisit after several days to confirm lifecycle actions.

**Expected Outcome**: Learn how to automate data archiving and cleanup in S3.
