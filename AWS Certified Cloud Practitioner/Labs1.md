

# AWS Certified Cloud Practitioner Hands-On Labs Guide

Welcome to your comprehensive hands-on lab guide for the **AWS Certified Cloud Practitioner (CCP)** exam. These labs are drawn from some of the most trusted open-source resources – including community repositories by AWS experts (such as Chandra Lingam, Monil Parmar, Paarth Pandey, Minh Hung Phan, and Sourav Pakhira) – to ensure you gain practical experience on core AWS services. We have designed these step-by-step exercises to introduce you to the AWS cloud environment and let you **experience the benefits first-hand**. By following this guide, you'll build confidence in using the AWS Management Console and CLI, aligning your skills with **CCP exam domains** (Cloud Concepts, Security and Compliance, Technology, and Billing/Pricing).

**How to Use this Guide:** Each lab below is organized by AWS service category (and mapped to the relevant exam domain). Follow the instructions carefully, and perform the steps in your own AWS account. **Notes** are included to explain key points. **Validation steps** help you verify the outcome, and **cleanup instructions** are provided to delete or terminate resources to avoid unnecessary costs. (Remember, you are responsible for any AWS costs incurred during these labs – all tasks use the AWS Free Tier where possible, but always double-check your usage.)

## Prerequisites and Setup

Before you begin, make sure you have the following:

* **AWS Account:** An active AWS account with admin-level access. If you don’t have one, [create a free AWS account here](https://aws.amazon.com/free/). Ensure you are familiar with the AWS Management Console interface.
* **AWS CLI (Optional):** Some labs include AWS Command Line Interface steps. Install and configure the AWS CLI on your system if you plan to do CLI-based operations. Set up your AWS Access Key ID and Secret Access Key for the CLI (from the IAM section of your AWS account) and run `aws configure` to initialize your credentials and default region.
* **Free Tier Awareness:** Whenever possible, select **free-tier eligible** resources (e.g. t2.micro EC2 instances, 5 GB of S3 storage, etc.). This will help avoid charges. All labs are designed to stay within free usage limits or incur minimal cost.
* **Basic AWS Knowledge:** A fundamental understanding of what AWS is and the purpose of services like EC2, S3, etc. will help, but the labs will also teach you by doing.

> *Note:* It’s recommended to perform these labs in a personal AWS account (not a production environment). Also, consider using an IAM user rather than root credentials for routine tasks, following AWS security best practices.

---

## Identity and Access Management (IAM) – *Domain: Security & Compliance*

AWS Identity and Access Management (IAM) is the service that controls **user access and permissions** in AWS. In the CCP exam’s Security domain, you need to understand the basics of IAM (users, groups, roles, and policies). The following lab will give you practical experience with IAM using the AWS Console.

### Lab 1: Creating an IAM User and Group (Console)

**Objective:** Set up a new IAM user with limited permissions by placing it in a group and attaching a policy. This reflects real-world scenarios where you grant least privilege access instead of using root credentials.

**Steps:**

1. **Navigate to IAM:** Log in to the AWS Management Console with your root or administrative account. From the AWS services menu, go to **IAM (Identity and Access Management)**.
2. **Create a Group:** In the IAM console, click **User groups** in the sidebar, then click **Create group**. Enter a group name (e.g., "`CCP-Lab-Users`").

   * Under *Attach permissions policies*, check the box for a policy to grant the group permissions. For this lab, select **AmazonS3ReadOnlyAccess** (as an example of a limited permission). This policy lets users list and get objects from S3 buckets without giving full admin rights.
   * Click **Create group**. The group should now appear in the User groups list.
3. **Create a User:** Next, click **Users** in the IAM sidebar, then **Add users**. Enter a username (e.g., "`test.user`), and select **AWS Credential Type – Password** for console access (so you can log in via the console as this user). You can check **Auto-generate password** and note the generated password (or set a custom one).

   * *Optional:* For programmatic access, you could also tick **Access key - AWS CLI/SDK** to generate an access key and secret (not needed for console login in this lab).
4. **Assign User to Group:** On the *Set permissions* step of user creation, choose **Add user to group**, then select the "`CCP-Lab-Users`" group you created. This will automatically grant the new user the policies attached to that group (S3 read-only access in this case).

   * Skip adding tags. Review the user details and click **Create user**.
5. **Record Credentials:** After creation, you’ll see a success page. If you created a password, copy the login link (which will be in the format *https\://\<account\_id>.signin.aws.amazon.com/console*) and note the username and password. If an access key was created, download the CSV. (Treat these credentials securely.)
6. **Validate Access:** Test logging in as the new IAM user. Open an incognito/private browser window and navigate to the AWS login URL you copied. Log in with the new username and password. You should reach the console. Try accessing an AWS service to verify permissions:

   * For example, go to **S3** service. You should be able to list S3 buckets (since we gave read-only access to S3). Try to **create a new bucket** – this should **fail with an Access Denied** error, because the user only has read permissions. This confirms the policy is in effect.
   * Observe that you cannot access other services like EC2 or IAM (you’ll get permission denied), since we did not grant those rights.
7. **Cleanup – Remove User and Group:** Sign out of the test user and sign back in as your admin user. In the IAM console, remove the resources to avoid clutter:

   * Go to **Users**, select the `test.user` you created. Choose **Delete user** from the Actions dropdown. Confirm deletion (this will also invalidate any access keys for that user).
   * Go to **User groups**, select `CCP-Lab-Users`, and choose **Delete group**. (You must remove any attached policies first: select the group, go to the **Permissions** tab, detach the AmazonS3ReadOnlyAccess policy, then delete the group.)

**End Result:** You have created a new IAM user and group with a specific access policy, verified the restricted permissions, and then cleaned up. This lab demonstrates IAM best practices like **using groups and managed policies to assign permissions** instead of working as root. It reflects the CCP exam emphasis on understanding IAM roles, users, groups, and policies in a practical way.

> *Note:* In real environments, you might keep IAM users rather than delete them. Here we clean up for a tidy lab environment. Always enforce MFA for IAM users with console access (not shown in this basic lab) and grant only necessary privileges.

---

## Compute Services – *Domain: Technology*

In the Technology domain of the exam, you must understand core compute services like Amazon EC2 (virtual servers on AWS) and AWS Lambda (serverless functions). The labs below cover launching an EC2 instance and deploying a Lambda function, giving you hands-on familiarity with how AWS compute works in practice.

### Lab 2: Launching an EC2 Instance (Amazon EC2 Web Console)

**Objective:** Learn how to launch a virtual machine (EC2 instance) in AWS. We will create a basic Amazon Linux 2 instance, configure networking (security group), connect to it, and then terminate it. This covers fundamental EC2 concepts: AMIs, instance types, key pairs, security groups, and the AWS region/AZ model.

**Steps:**

1. **Navigate to EC2:** Log in to the AWS Console and go to **EC2** (under Services > Compute > EC2).
2. **Start Instance Launch Wizard:** Click **Launch Instance**. In the "Launch an Instance" wizard:

   * **Name and Tags:** Give your instance a name, e.g., "`MyCCPTestInstance` (naming helps identify it later).
   * **AMI (Amazon Machine Image):** Choose **Amazon Linux 2 AMI (HVM), SSD Volume Type** – which is free tier eligible. Ensure the architecture is x86\_64.
   * **Instance Type:** Select **t2.micro** (default, free-tier eligible).
   * **Key Pair (Login):** If you have an existing key pair, select it. If not, create a new key pair (choose RSA, 2048 bits) named "`CCP-keypair`" and **Download Key Pair** (.pem file). This key is used for SSH access. Keep it secure.
   * **Networking:** Under *Network settings*, ensure the **VPC** is the default VPC and **Subnet** is set to a default subnet (e.g., us-east-1a). For *Auto-assign Public IP*, leave it enabled (to get a public IP for SSH).
   * **Firewall (Security Group):** Choose **Create security group** and name it (e.g., "`CCP-EC2-SG`"). It should have an **Inbound rule** for SSH:

     * Type: SSH, Port: 22, Source: Anywhere (0.0.0.0/0) – this allows you to connect from any IP. (In real scenarios, you would restrict this to your IP for security.)
   * **Storage:** The default 8 GB EBS gp2 volume is fine. Ensure "Delete on termination" is checked.
   * Leave advanced details at defaults. Review the summary.
3. **Launch the Instance:** Click **Launch Instance**. AWS will begin provisioning. Within seconds, you should see a success message and an Instance ID. Click **View all instances** to go to the EC2 Instances page.
4. **Verify Instance Running:** On the Instances page, find your new instance (`MyCCPTestInstance`). The **State** should transition to **running** (refresh if needed). Note the **Public IPv4 address** and **Availability Zone** in the details – this shows which data center your instance is in.
5. **Connect to the Instance:** We will use **EC2 Instance Connect** (browser-based SSH) for simplicity:

   * Select the instance, then click **Connect** > **EC2 Instance Connect** > **Connect**. This opens a browser terminal session to your Linux server.
   * If prompted, use the username `ec2-user` (for Amazon Linux). You should now see a welcome banner and a shell prompt on the EC2 instance.
   * Run a test command, for example: `uname -r` (to print the Linux kernel version) or `echo "Hello from EC2"` to confirm you can execute commands on the server.
   * *Alternate:* If Instance Connect fails (e.g., due to network), you can use a local SSH client with the downloaded `.pem` key: e.g., `ssh -i "CCP-keypair.pem" ec2-user@<Public-IP>`. Make sure to `chmod 400` the key file and use the correct public IP.
6. **Explore (Optional):** While connected, you can try installing a simple software to see the instance in action:

   * For example, update packages: `sudo yum update -y` and install Apache: `sudo yum install -y httpd`.
   * Start Apache: `sudo systemctl start httpd`. Then exit the SSH session.
   * In the EC2 console, find the instance’s public IP and try opening it in a web browser (http\://<instance-ip>). If Apache installed correctly, you should see the Apache test page. (This confirms outbound internet worked to install packages, and inbound HTTP would work if port 80 was open. In our case, port 80 isn’t in the security group, so you might not see a page – this is just an exercise in interacting with your instance.)
7. **Cleanup – Terminate Instance:** After you're done, **terminate** the EC2 instance to avoid charges:

   * In the EC2 console Instances list, right-click your instance (or select Actions > Instance State > Terminate). Confirm termination. The instance state will go to shutting-down, then terminated.
   * This also deletes the attached EBS volume (because “Delete on termination” was enabled). The security group and key pair remain in your account. You can reuse the key pair for future instances, and the security group can be deleted or left (no cost for having it with no instances).

**End Result:** You have launched a Linux virtual machine in AWS, connected to it, and then terminated it. You practiced configuring an EC2 instance with key pairs and security groups – fundamental tasks for any AWS cloud practitioner. Through this lab, you saw how AWS provides on-demand compute resources via EC2, reinforcing core concepts for the exam.

> *Note:* EC2 is a foundational service. In the real exam, you should know that EC2 provides resizable compute capacity and understand related concepts (instance types, regions/AZs, security groups, EBS volumes, etc.), which this lab has demonstrated in practice.

### Lab 3: Deploying a Serverless Function (AWS Lambda Console)

**Objective:** Get hands-on experience with AWS Lambda – a serverless compute service. We will create a simple Lambda function in the console, invoke it manually, and view logs in CloudWatch. This lab solidifies understanding of how Lambda lets you run code without managing servers, a concept covered in the exam's Technology domain.

**Steps:**

1. **Navigate to Lambda:** In the AWS Console, go to **Lambda** (under Services > Compute > AWS Lambda).
2. **Create Function:** Click **Create function**. Choose **Author from scratch**. Enter a function name, e.g., "`HelloLambdaCCP`".

   * Runtime: Select **Python 3.9** (or the latest Python available).
   * Role: For this simple test, choose **Create a new role with basic Lambda permissions**. This will create an IAM role for the Lambda with permission to upload logs to CloudWatch. (No special access to other services needed for now.)
   * Click **Create function**. Lambda will set up the function, which may take a few seconds.
3. **Write the Code:** Once created, you’ll see the function’s configuration page. Scroll to the **Code Source** section. It may already have a sample handler. Replace the code with the following Python code (you can use the built-in code editor):

   ```python
   def lambda_handler(event, context):
       # This is the entry point for the Lambda function
       message = "Hello, AWS Cloud Practitioner!"
       print(message)  # log message to CloudWatch
       return {
           'statusCode': 200,
           'body': message
       }
   ```

   This code simply prints a greeting and returns it in an HTTP-style response object.

   * Click **Deploy** to save the code.
4. **Test the Function:** We’ll now invoke the Lambda manually:

   * Click **Test** in the Lambda console toolbar. For the first test, you'll need to configure a test event. Select **“Create new test event”**, choose the template “Hello World” (any dummy event is fine), and name it “TestEvent”. Then click **Save**.
   * Now click **Test** again to invoke the function with that sample event.
   * You should see the result show up in the console: a green banner with "Execution succeeded" and the returned result in JSON (statusCode 200 and the body message). Below that, the **Log output** section will show the printed message and other log info. For example:

     ```
     "Hello, AWS Cloud Practitioner!"
     END RequestId: ... 
     REPORT RequestId: ... Duration: ... ms ...
     ```

     This indicates our function ran and logged the greeting to CloudWatch Logs.
5. **View Logs (Optional):** Click on **Monitor** > **View logs in CloudWatch** to see the log group for this Lambda. You should find the log stream for the recent invocation containing the printed message. (This step verifies that CloudWatch integration is working, which is automatic with the basic Lambda execution role.)
6. **Cleanup – Delete Function:** If you won’t use this function again, delete it to avoid any confusion or accidental usage:

   * On the function page, choose **Actions > Delete function**, then confirm by typing “delete”. This will remove the Lambda function and its code.
   * The IAM role created for the function (e.g., one starting with `HelloLambdaCCP-role-...`) remains. You can delete that role via IAM console for completeness: go to IAM > Roles, find the role and ensure no other functions use it, then delete it. (Optional, as an unused role has no cost.)

**End Result:** You created a serverless function that runs on-demand without provisioning any server. You manually invoked it and saw output logs. This covers how Lambda functions are configured and tested. It also reinforces the concept of AWS handling execution behind the scenes (you were not concerned with launching or managing any servers for this code).

> *Note:* In a real scenario, you might connect this Lambda to triggers (e.g., an API Gateway endpoint or an S3 event) rather than invoking manually. For the CCP exam, understand that Lambda is a compute service where you pay per execution time, and it scales automatically. Our simple example printed a message, but Lambdas can do much more. Logs for Lambda functions go to CloudWatch automatically – we verified that in this lab.

---

## Storage Services – *Domain: Technology*

AWS storage is another key area. Amazon S3 (Simple Storage Service) is a foundational service for object storage, heavily featured in the CCP exam. This lab will walk through creating an S3 bucket, uploading data, enabling website hosting, and understanding access settings.

### Lab 4: Amazon S3 Bucket Operations and Static Website Hosting

**Objective:** Practice using Amazon S3 by creating a bucket, uploading a file, making it public, and enabling static website hosting. This demonstrates S3’s core capabilities and how to host a simple static website – a use case often discussed at the practitioner level.

**Steps:**

1. **Navigate to S3:** In the AWS Console, go to **S3** (under Services > Storage > S3).
2. **Create a Bucket:** Click **Create bucket**. Provide a globally unique bucket name (e.g., "`ccp-lab-<your-initials>-website`"). Choose an AWS region close to you. Leave other settings as default *except* uncheck **"Block *all* public access"** for this bucket – since we intend to host a public website, we need to allow public access. Acknowledge the warning about public access.

   * Keep Versioning disabled (for now) and default encryption enabled or disabled as you prefer (not critical for this lab). Click **Create bucket**.
3. **Upload an Object:** Click your new bucket name to enter it. Click **Upload** > **Add files** and select a small HTML file from your computer to upload (e.g., create a simple `index.html` that says "Hello from S3"). You can also use a text editor now to create a basic `index.html` locally containing `<h1>Hello from AWS S3</h1>` and upload that file.

   * In upload options, under **Permissions**, since we want this file public, expand **Permissions** and check the box **"Grant public-read access"**. (If you don’t see this, it might be because blocking public access is still on. Ensure you disabled block public access in step 2.)
   * Complete the upload. The file should appear in the bucket.
4. **Enable Static Website Hosting:** In the bucket properties, scroll to **Bucket settings for static website hosting**. Click **Edit** (or **Enable** depending on console version). Choose **Enable** and select "Host a static website". For **Index document**, enter `index.html`. (We won’t use an error document for now.) Save changes.

   * The console will display a **Bucket website endpoint**, a URL where your bucket’s content will be served (something like `http://<bucket-name>.s3-website-<region>.amazonaws.com`).
5. **Test Public Access:** Now let’s retrieve the file via the web:

   * Click the object `index.html` in the bucket, and look at its **Object URL**. It should be an S3 URL (possibly ending in `.amazonaws.com/<bucket>/<object>`). Open that URL in a new browser tab. You should see your HTML page content (the "Hello from AWS S3" message).
   * Alternatively, use the **bucket website endpoint** URL (it should automatically serve `index.html`). Go to the bucket website endpoint in a browser – it should display the same page. This confirms that public access is working and the bucket is functioning as a static website host.
   * If you get an error (403 Forbidden), double-check that **Block Public Access** is off and that the object has **Public** listed next to it in S3 console. You might need to add a Bucket Policy allowing public read. For example, a bucket policy:

     ```json
     {
       "Version": "2012-10-17",
       "Statement": [{
         "Effect": "Allow",
         "Principal": "*",
         "Action": "s3:GetObject",
         "Resource": "arn:aws:s3:::ccp-lab-<your-initials>-website/*"
       }]
     }
     ```

     This policy (with your bucket name) would open read access. Attach it in the **Permissions** tab of your bucket if needed. (Many S3 website tutorials include this step.)
6. **Notes:** You have now stored an object in S3 and accessed it via the internet. Remember, by default **new S3 buckets block public access** for safety. We intentionally enabled public access here for a benign static site. In practice, sensitive data in S3 should remain private (accessible only by authorized IAM principals or through CloudFront, etc.). S3 is highly durable storage for objects and is commonly used to host websites, store backups, or act as data lakes.
7. **Cleanup – Remove S3 Assets:** To avoid leaving public data:

   * Delete the object: Go into the bucket, select `index.html`, click **Delete**, and confirm.
   * Delete the bucket: Go back to **Buckets** list, select your bucket, and choose **Delete**. You must type the bucket name to confirm. The bucket will be deleted. (If you forget to delete the object first, the bucket deletion will fail; ensure the bucket is empty.)

**End Result:** You created an S3 bucket, adjusted its access settings, uploaded a file, and successfully accessed that file via a public URL. This lab taught you how S3 hosting works and the importance of permission settings. It reinforces exam topics around S3 basics (what a bucket is, how to make content public, static site hosting, etc.).

> *Note:* S3 is a **foundational service** in AWS and the CCP exam. Key points include: S3 stores objects (not filesystems), it’s region-specific but accessed via global endpoints, and by default objects are private. You also saw that S3 can serve static websites (an exam-worthy tidbit). Always ensure you **clean up public buckets** or restrict access when done, to follow security best practices.

---

## Networking – *Domain: Technology*

Amazon VPC (Virtual Private Cloud) allows you to create isolated virtual networks in AWS. The CCP exam expects you to grasp what a VPC is and how subnets, internet gateways, and related components work at a high level. In this lab, we'll set up a custom VPC with a public subnet and launch an EC2 instance inside it to verify connectivity. (This is a bit more advanced, but it ties together concepts of networking and EC2.)

### Lab 5: Building a Custom VPC with a Public Subnet

**Objective:** Create a new VPC (instead of using the default VPC), set up a subnet, Internet Gateway, and route table, then run a simple EC2 instance in that subnet. This hands-on exercise will demystify how AWS networking is configured. It aligns with exam topics on VPC basics and Internet connectivity.

**Steps:**

1. **Navigate to VPC Service:** Open the AWS Console and go to **VPC** (under Services > Networking & Content Delivery > VPC).
2. **Create a VPC:** In the VPC Dashboard, click **Your VPCs** > **Create VPC**. Choose **VPC Only** (or **VPC and more** if it offers a wizard – we can use VPC only for manual control):

   * Name tag: "`CCP-Lab-VPC`".
   * IPv4 CIDR block: "`10.0.0.0/16`" (this gives up to 65,536 addresses, plenty for our example).
   * IPv6 CIDR: No IPv6 for simplicity (None).
   * Tenancy: Default.
   * Click **Create VPC**. You should see the new VPC in your VPC list, with the CIDR 10.0.0.0/16.
3. **Create a Subnet:** Now add a subnet inside this VPC. Go to **Subnets** > **Create subnet**.

   * Select **CCP-Lab-VPC** as the VPC.
   * For Subnet settings, name it "`CCP-Public-Subnet`", choose an AZ (e.g., us-east-1a) and IPv4 CIDR "`10.0.1.0/24`" (this gives 256 IPs for this subnet).
   * Click Create. The subnet will appear in the list, associated with the VPC.
   * By default, this subnet is not "public" (no internet access yet). To make it public, we need to attach an Internet Gateway and update route tables.
4. **Create and Attach Internet Gateway (IGW):** In the VPC sidebar, click **Internet Gateways** > **Create internet gateway**.

   * Name it "`CCP-Lab-IGW`" and Create.
   * Select the new IGW in the list, choose **Actions > Attach to VPC**, and select **CCP-Lab-VPC**. Attach. The IGW state should change to "attached". This IGW will allow traffic between the VPC and the internet.
5. **Route Table for Public Subnet:** When the VPC was created, AWS also created a **default route table** for it. We will use that for our public subnet or create a new one:

   * Go to **Route Tables** in the VPC console. Filter by VPC = CCP-Lab-VPC. You should see one route table (let’s call it *RT-1* for reference). It’s likely the Main route table for the VPC.
   * Select that route table, and in the details below click **Routes**. You should see the default local route for 10.0.0.0/16 (allowing internal VPC traffic). Click **Edit routes** and then **Add route**:

     * Destination: `0.0.0.0/0` (this means "any IPv4 address on the internet").
     * Target: choose **Internet Gateway** and select `CCP-Lab-IGW`.
     * Save routes. Now the route table has an entry sending all non-local traffic to the IGW.
   * Next, associate this route table with our subnet to make it effective there. Click the **Subnet Associations** tab for the route table. Click **Edit subnet associations**, check the box for `CCP-Public-Subnet`, and save. This associates the subnet with the route table that has the IGW route, effectively making it a **public subnet**.
6. **Enable Auto-Assign Public IP:** One more setting: by default, new subnets might not automatically assign public IPs to instances. To allow instances in this subnet to get a public IPv4:

   * Go to **Subnets**, select `CCP-Public-Subnet`. Choose **Actions > Edit subnet settings** (or in some UIs it's an attribute on the subnet). Enable **Auto-assign IPv4 public subnet** and save. This ensures any EC2 in this subnet gets a public IP by default.
7. **Launch EC2 in the New VPC:** Now we’ll deploy a small EC2 instance in this network to test connectivity.

   * Go to **EC2** service > Instances > Launch Instance. Name it "`VPC-Test-Instance`". Choose Amazon Linux 2 AMI, t2.micro, etc., as in the earlier EC2 lab.
   * In the **Network settings** section, choose **CCP-Lab-VPC** as the VPC, and **CCP-Public-Subnet (us-east-1a)** as the subnet.
   * Ensure **Auto-assign public IP** is enabled (should reflect what we set).
   * For **Firewall (security group)**, create a new security group (specific to this VPC) e.g. "`CCP-VPC-SG`" allowing SSH (port 22 from anywhere) similar to before. (The default security group of this VPC could also be used if you allowed inbound, but we'll make a new one.)
   * Choose the same key pair as before (you can reuse `CCP-keypair`).
   * Launch the instance.
   * In a minute, verify the instance is running. Note its **Public IPv4** address.
8. **Test Connectivity:** Use EC2 Instance Connect or your SSH client to connect to the instance (just as we did in Lab 2).

   * If connected, try `ping 8.8.8.8` (a public DNS server) from the instance or `curl http://example.com`. It should succeed, indicating the instance has internet access via the IGW. (Ping might be blocked by security group egress rules or OS firewall for ICMP, but `curl` should fetch HTML.)
   * This confirms that our IGW and route are functioning, and the subnet is truly public.
9. **Cleanup – Delete VPC Resources:** This is important to avoid leaving idle resources:

   * Terminate the EC2 instance (as in Lab 2 cleanup). Wait for it to fully terminate.
   * Detach and delete the Internet Gateway: In VPC > Internet Gateways, select `CCP-Lab-IGW`, Actions > Detach from VPC (choose the VPC, detach), then Actions > Delete IGW.
   * Delete the subnet: In VPC > Subnets, select `CCP-Public-Subnet`, Actions > Delete. (You cannot delete a subnet until instances in it are gone, which we ensured by terminating the EC2.)
   * Delete the VPC: In VPC > Your VPCs, select `CCP-Lab-VPC`, Actions > Delete. Confirm. This will also remove the default route table and any other default components for that VPC. (If an error occurs, ensure all EC2 instances, subnets, and gateways for that VPC are removed as above.)

**End Result:** You manually built a VPC with a public subnet and confirmed that an EC2 instance within it could reach the internet. This lab ties together networking components: **VPC**, **subnet**, **route table**, **Internet Gateway**, and **security group**. It provides insight into how the default VPC is configured (default VPCs have a public subnet and IGW set up similarly).

> *Note:* For the CCP exam, you should recognize that a **VPC** is an isolated network in the AWS cloud. Every instance runs inside a VPC. Public subnets require an IGW and route 0.0.0.0/0 to that IGW. Private subnets route internet traffic through NAT Gateways or not at all. We didn't cover NAT here, but keep in mind that concept for higher-level exams. The key takeaway is that **VPCs allow you to control network address ranges and traffic flow in AWS** – and now you have hands-on context for those concepts.

---

## Databases – *Domain: Technology*

AWS offers managed database services like Amazon RDS for relational databases. The CCP exam expects knowledge of RDS at a basic level (what it is, use cases, and that it’s managed by AWS). In this lab, we will create a small MySQL database instance in Amazon RDS, connect to it using a simple method, and then clean up. This demonstrates how easy it is to set up a database in AWS without managing a server yourself.

### Lab 6: Creating a MySQL Database Instance with Amazon RDS

**Objective:** Launch an Amazon RDS MySQL instance (free tier), connect to it using AWS’s web-based query tool, and then delete it. You will learn about DB instance settings like credentials, multi-AZ, security groups, etc., at a practical level.

**Steps:**

1. **Navigate to RDS:** In the AWS Console, go to **RDS** (under Services > Databases > RDS).
2. **Launch a DB Instance:** Click **Create database**. For database creation method, choose **Standard Create**.

   * Engine options: Select **MySQL** and Version (choose a recent MySQL version that is Free Tier eligible, e.g., MySQL 8.x Community).
   * Template: Select **Free tier**.
   * Settings: Choose an easy **DB instance identifier** (e.g., "`ccp-mysql-db`"). Set **Master username** to "`admin`" (or any name you like), and **Master password** – enter a password and confirm it (remember this or note it securely).
   * Instance configuration: Choose **db.t2.micro** (should be default for free tier).
   * Storage: 20 GiB (General Purpose SSD) default is fine. (Free tier allows up to 20 GiB.)
   * **Availability & durability:** For free tier, Multi-AZ is not included, so keep it unchecked.
   * Connectivity: **VPC** should default to your default VPC. **Subnet group:** default. **Public access:** **Yes** – select "Publicly accessible" to easily connect (in real scenarios you might keep databases private, but for this lab and since we will delete it, we allow public for test).

     * **VPC security group:** Choose **Create new security group** (unless a suitable one exists). It will name it like `ccp-mysql-db-sg`. This will by default allow the port for MySQL (3306) from anywhere or from the current IP (the console might auto-configure to your IP; if not, we may edit it later).
   * Additional configurations:

     * **Initial database name:** (Optional) enter "`testdb`" so that RDS creates a default database inside MySQL for us.
     * Backup, monitoring, maintenance: for a short lab, you can leave these defaults (backups 7 days etc.).
   * Review all settings are free-tier compatible (there should be an indicator if not). Then click **Create database**.
3. **Wait for Creation:** The new DB instance status will be **Creating**. It may take several minutes (\~5-10 minutes) to be available.

   * While waiting, in the RDS **Databases** list, observe the **Endpoint** address that appears for your DB. It will be something like `ccp-mysql-db.xxxxxx.<region>.rds.amazonaws.com`. This is the address to connect to the database.
   * Also note the **VPC security group** associated. If needed, click on it to verify the inbound rule. Ideally it should allow MySQL (port 3306) from your IP. If not, you can edit the SG in the EC2 console to add an inbound rule for MySQL from your IP or 0.0.0.0/0 (for testing only). Be careful: opening to 0.0.0.0/0 means anyone can attempt to connect if they have your endpoint and password.
4. **Connect using Query Editor (in Console):** AWS RDS provides an online query editor for convenience (for MySQL and Aurora):

   * In the RDS console, click **Databases**, then your database instance. On the **Connectivity & security** tab, note if Public access is yes and status is available.
   * On the **Actions** menu for the DB instance, you may see **Manage query editor** or **Launch query editor**. If available, click **Connect** (or Query Editor). This opens the Query Editor in your browser.
   * You might need to log in to the query editor with database credentials: select the database (`ccp-mysql-db`), user as `admin`, and enter the master password you set. Click **Connect**.
   * Once connected, ensure the default database is `testdb` (if you created it). You can run a test SQL query:

     ```sql
     CREATE TABLE SampleTable (ID INT PRIMARY KEY, Name VARCHAR(50));
     SHOW TABLES;
     ```

     This will create a simple table and list tables. The output should show `SampleTable`. This confirms you can execute queries on the MySQL database.
   * You have now interacted with the database without installing any MySQL software locally – using AWS’s tools.
5. **Cleanup – Delete the Database:** We will delete the RDS instance to avoid ongoing costs:

   * Exit the query editor and go back to the RDS console **Databases** page. Select your `ccp-mysql-db` instance, and click **Actions > Delete**.
   * It will ask whether to take a final snapshot. Since this is just a lab, select **No** for final snapshot (check the box to acknowledge you want to delete without snapshot). Also acknowledge the option to retain automated backups (you can disable that for immediate deletion).
   * Type "delete me" or the database identifier as requested to confirm, and proceed with **Delete**. The status will go to "deleting" and within a few minutes, the instance will be removed.
   * Also remove the security group if it’s no longer needed (find `ccp-mysql-db-sg` in EC2 > Security Groups, and delete it if not in use by any other resource).

**End Result:** You created a managed MySQL database instance and connected to it using AWS’s RDS query editor. This demonstrates RDS’s ease of setup: you didn’t have to install MySQL on an EC2 server or manage OS patches – AWS handled the heavy lifting. You also saw how to make the database publicly accessible for testing (in practice, databases are often in private subnets).

> *Note:* For the exam, remember that **Amazon RDS is a managed relational database service** supporting engines like MySQL, PostgreSQL, Oracle, SQL Server, etc. AWS manages backups, patching, high availability (with Multi-AZ deployments), and scaling under the hood. You should also know that RDS instances reside in a VPC and use security groups for network access control (as we configured). We used the free tier (Single-AZ, db.t2.micro) for cost efficiency. Always delete or stop RDS instances when not needed to avoid charges.

---

## Monitoring & Logging – *Domain: Technology*

AWS CloudWatch is the main service for monitoring resources and logging. The CCP exam covers basic CloudWatch concepts (metrics, alarms, and the idea of CloudWatch Logs). In this lab, you will create a CloudWatch alarm for an EC2 instance’s CPU utilization and (optionally) observe how it would trigger. This gives you practical insight into setting thresholds and notifications for resource metrics.

### Lab 7: Creating a CloudWatch Alarm for EC2 CPU Utilization

**Objective:** Set up an Amazon CloudWatch Alarm on an EC2 instance’s CPU usage metric. We will trigger it manually by stressing the CPU (optional), and discuss how alarms send notifications. This hands-on exercise reinforces CloudWatch’s role in automated monitoring.

**Steps:**

1. **Setup an EC2 Instance to Monitor:** If you have an EC2 instance running from earlier labs (e.g., the one in the custom VPC or a new t2.micro in default VPC), note its Instance ID. If not, launch a small EC2 (t2.micro Amazon Linux) now in the default VPC for this exercise (you can terminate it after).

   * *Tip:* To keep it simple, launch it in the default VPC and open SSH (no need for public IP if you use Session Manager, but for ease, you can allow SSH from your IP). We will use it to potentially generate CPU load.
2. **Navigate to CloudWatch:** Go to **CloudWatch** in the AWS Console (under Services > CloudWatch).
3. **Create an Alarm:** In the CloudWatch console, click **Alarms** > **All alarms** > **Create alarm**.

   * Click **Select metric**. We want EC2 metrics, so choose **EC2** > **Per-Instance Metrics**. Find and select **CPUUtilization** for the instance you want to monitor (you may need to find your instance ID in the list).
   * The metric graph will show on the right. Click **Select metric** to proceed.
   * **Configure trigger:** Set the conditions for the alarm:

     * Period: 5 minutes (default).
     * Statistic: Average.
     * Condition: e.g., "Greater than 50". So it reads: Trigger alarm when **Average of CPUUtilization > 50%** for a single period.
     * This means if CPU usage goes above 50% at a 5-minute average, the alarm will go to ALARM state.
   * Click **Next**.
   * **Notification (Alarm actions):** For this lab, we can skip sending notifications (choose **No notification**). In real setups, you’d create an SNS topic to email or SMS you. (If you want to see it, you can set up an SNS topic and subscription, but it requires email/phone confirmation.)
   * Click **Next**.
   * **Alarm Name:** Give it a name like "`HighCPUAlarm-CCP`". Description optional.
   * Review and **Create alarm**.
4. **Observe Alarm State:** After a few minutes, go to CloudWatch **Alarms** section and find your alarm. It will likely be in **OK** state initially (if the instance CPU is below 50%). The alarm is now actively monitoring that instance metric.
5. **(Optional) Trigger the Alarm:** To see the alarm change state, you can induce high CPU on the instance:

   * Connect to the EC2 (via SSH or EC2 Instance Connect).
   * Run a CPU stress command. If you have root access, you might install `stress` or simply do: `yes > /dev/null &` to start a process that uses 100% of a CPU core. You might run a couple of those background `yes` commands to push usage above 50%.
   * Let it run for a few minutes. CloudWatch metrics are collected every 5 minutes by default for free (1-minute intervals require detailed monitoring). Our alarm period is 5 minutes, so after one period of high usage, the alarm should fire.
   * Back in CloudWatch Alarms, you should see the status change to **ALARM** (it may take 5-10 minutes to refresh). If you had an SNS notification, it would send at this point.
   * This step demonstrates how CloudWatch alarms respond to metric breaches.
6. **Stop the CPU stress (if done):** Terminate the `yes` processes on your instance (`kill $(pgrep yes)` to stop them, or simply close the instance if you plan to terminate it).

   * The alarm will eventually return to **OK** state after the CPU drops below 50% for one period.
7. **Cleanup – Delete Alarm (and instance):**

   * In CloudWatch, select the alarm and **Actions > Delete**. Confirm deletion. This removes the alarm (no cost for alarms generally, but it’s good practice to clean up).
   * Terminate the EC2 instance if it was only created for this lab.
   * If you set up an SNS topic for notifications, you may delete that topic as well via the SNS console to avoid clutter.

**End Result:** You created a functioning CloudWatch alarm for an EC2 metric. By optionally triggering it, you observed how CloudWatch monitors metrics and changes alarm state. Even if you didn't force it to alarm, you have configured one and know how it would behave. This experience covers CloudWatch concepts: metrics (CPUUtilization), alarm thresholds, periods, and actions.

> *Note:* In the CCP exam, you should understand that **CloudWatch** collects metrics from AWS services and you can set alarms on those metrics. CloudWatch alarms can automatically trigger notifications or even autoscaling actions (in advanced cases). Also, CloudWatch includes **CloudWatch Logs** for aggregating logs from services – we indirectly saw that with Lambda logs earlier. Remember that CloudWatch by default has 5-minute metric intervals for EC2 (free), and 1-minute detailed monitoring costs extra (we used the default). Setting an alarm is a practical skill that shows you how AWS can automate monitoring for you.

Additionally, be aware of **AWS CloudTrail** (logs API calls for governance) – not directly used in this lab, but part of monitoring/security in AWS. CloudTrail is enabled by default for account activity logging to S3/CloudWatch and is another tool mentioned in CCP exam, complementing CloudWatch.

---

## Infrastructure as Code – *Domain: Technology*

AWS CloudFormation is the service that allows you to provision infrastructure by writing templates (in JSON/YAML). While CCP exam expectations on CloudFormation are light, it's good to have a basic understanding. In this lab, you will use a simple CloudFormation template to create an S3 bucket, demonstrating how infrastructure can be deployed as code.

### Lab 8: Deploying Resources with AWS CloudFormation (S3 Bucket Example)

**Objective:** Use CloudFormation to deploy an AWS resource (an S3 bucket) and then tear it down, all through a template. This will illustrate the infrastructure-as-code concept: rather than clicking in the console to create resources, you describe them in a template and let CloudFormation handle the creation and configuration.

**Steps:**

1. **Prepare a Template:** Below is a very simple CloudFormation **YAML** template that defines an S3 bucket resource:

   ```yaml
   AWSTemplateFormatVersion: '2010-09-09'
   Description: Simple CloudFormation template to create an S3 bucket for AWS CCP lab.
   Resources:
     MySampleBucket:
       Type: AWS::S3::Bucket
       Properties:
         BucketName: ccp-cf-sample-bucket-<YOUR INITIALS>   # bucket name must be unique
         AccessControl: Private
   ```

   This template has one resource of type S3 Bucket. It specifies a BucketName (make sure to insert your initials or a unique string to avoid name clashes). The AccessControl is set to Private (the default, actually).
2. **Launch CloudFormation Stack:** Go to **CloudFormation** in the AWS Console (Services > CloudFormation).

   * Click **Create stack** > **With new resources (standard)**.
   * On the "Specify template" step, choose **Template is ready** and then select **Upload a template file**. Click **Choose file** and upload your YAML file (if you saved it locally), or copy-paste the template into a text file to upload. *(Alternatively, you can use the "Template editor" and paste the template directly by choosing "Template is ready" > "Edit template" and pasting the YAML. Ensure it validates.)*
   * Click **Next**.
   * Stack Name: give a name like "`CCP-S3Bucket-Stack`".
   * Parameters: (None for this template, since bucket name was hard-coded).
   * Click **Next**. For Configure stack options, no changes needed – click Next.
   * On Review, check the box to acknowledge that CloudFormation might create IAM resources (not needed for S3 but the UI might require acknowledgement for any capabilities).
   * Click **Create stack**.
3. **Monitor Stack Creation:** You will be taken to the Stack details page. The stack status will be **CREATE\_IN\_PROGRESS**. In the **Events** tab, you can see each step. It should create the S3 bucket. Within a minute or so, the status should become **CREATE\_COMPLETE**.

   * If creation fails (status **ROLLBACK\_IN\_PROGRESS**), likely the bucket name was not unique. Check the Events for an error like "bucket name already exists". If so, adjust the name in the template (the `<YOUR INITIALS>` part or add random digits) and try again with a new stack.
4. **Verify the Resource:** Once the stack is CREATE\_COMPLETE, go to the S3 console and verify that a bucket with the name you specified was created. It should be there. CloudFormation also shows the resource in the **Resources** tab of the stack (you’ll see `MySampleBucket` of type AWS::S3::Bucket and its physical ID which is the bucket name).
5. **Cleanup – Delete the Stack:** In CloudFormation, select your stack (`CCP-S3Bucket-Stack`) and click **Delete**. Confirm deletion. CloudFormation will then delete the S3 bucket as part of tearing down the stack (status will go to DELETE\_IN\_PROGRESS, then the stack will disappear from the list upon success).

   * **Important:** If you had put any objects into the bucket, the deletion might fail because CloudFormation cannot delete a non-empty bucket. In our case, we created it and left it empty, so deletion should succeed. (If it ever fails due to bucket not empty, you’d empty the bucket and retry delete.)
   * Verify in S3 console that the bucket is gone.

**End Result:** You deployed an S3 bucket using a CloudFormation template and then removed it by deleting the stack. This demonstrated the **infrastructure as code** approach. Instead of manually clicking through the S3 console, you described the desired state, and CloudFormation provisioned it for you.

> *Note:* For the CCP exam, know that **AWS CloudFormation** automates the provisioning of resources (infrastructure as code). While you won’t need to write templates in the exam, understanding the benefit (deploying consistent environments, automated teardown, source-controlled infrastructure definitions) is useful. In this lab, we kept it simple with one resource. CloudFormation templates can grow to manage complex deployments of dozens of resources across services. The key takeaway is that AWS can create and manage groups of resources on your behalf when you define them in a template, increasing reproducibility and reducing human error.

---

## Billing & Pricing – *Domain: Billing and Pricing*

A significant portion of the CCP exam covers AWS billing, pricing models, and how to estimate costs. Being able to use the **AWS Pricing Calculator** is a practical skill to estimate your cloud architecture costs. In this final lab, we will walk through using the AWS Pricing Calculator to estimate the monthly cost of a simple web application scenario. This will reinforce your understanding of the cost factors for various services.

### Lab 9: Estimating Costs with the AWS Pricing Calculator

**Objective:** Use the AWS Pricing Calculator to estimate the monthly cost of running a basic web application on AWS. We will include an EC2 instance, an RDS database, and some S3 storage in the estimate. Through this, you’ll learn how to navigate the calculator and interpret cost breakdowns – an essential skill for cost management questions on the exam.

**Steps:**

1. **Access AWS Pricing Calculator:** Open your web browser and go to the AWS Pricing Calculator at **[https://calculator.aws](https://calculator.aws)**. (It’s a web app; you do not need to log in, but you can log in with your AWS account to save estimates if desired.)
2. **Start a New Estimate:** Click **Create estimate** (you might see a default empty estimate if it's your first time).
3. **Add EC2 to the Estimate:** In the calculator, under **Services**, search for "EC2" or find **Amazon EC2** in the list and select it.

   * Configure the EC2 parameters for a hypothetical server:

     * **Region:** Choose a region (e.g., US East (N. Virginia)).
     * **Pricing model:** select **On-Demand** (default) – since as a Cloud Practitioner you should know on-demand is pay-as-you-go. (Spot and Reserved are options but we’ll stick to on-demand for simplicity.)
     * **Operating system:** e.g., Linux.
     * **Instance type:** say **t2.micro** (as in our labs, suitable for a small web server).
     * **Quantity:** 1 instance.
     * **Usage:** 24 hours/day, 7 days/week (the calculator might let you put 730 hours for a month).
     * **Storage:** Add storage – e.g., one General Purpose SSD volume of 30 GiB (for OS and app). The calculator will price EBS storage separately.
     * **Data transfer:** You can input a value for data out to the internet. For example, estimate **20 GB** out per month.
     * After entering these, on the right you should see a monthly cost estimate for this EC2 (it will break down into compute cost, storage cost, data transfer cost).
4. **Add RDS to the Estimate:** Now let’s add a database. Click **Add service** and select **Amazon RDS**.

   * **Engine:** choose **MySQL**.
   * **Deployment type:** Single-AZ (cheaper; Multi-AZ doubles cost for high availability).
   * **Instance type:** e.g., db.t3.micro.
   * **License model:** License included.
   * **Storage:** say 20 GiB GP2 (General Purpose SSD).
   * **Requests:** you can leave I/O requests as default or a small number.
   * **Multi-AZ:** No.
   * **Additional storage needs:** set backup storage maybe 5 GiB.
   * On the right, note the monthly cost for the RDS instance.
5. **Add S3 to the Estimate:** Click **Add service** and choose **Amazon S3**.

   * **Storage amount:** e.g., 50 GB (Standard storage).
   * **Requests:** the calculator may ask how many PUT, GET requests. For example, estimate 10,000 PUT and 50,000 GET per month.
   * **Data transfer out:** if any data served from S3 to internet, say 5 GB.
   * The calculator shows the cost (S3 is usually a few dollars for this usage).
6. **Review the Total Estimate:** Now you have three services in your estimate (EC2, RDS, S3). The calculator interface typically shows a **summary** of monthly costs:

   * EC2 might be on the order of \~\$8-\$10/month for t2.micro 24/7 plus storage.
   * RDS t3.micro maybe \~\$15-\$20/month.
   * S3 50GB + requests maybe a few dollars.
   * The **total** might be around \$30-\$40/month for this entire setup (these are rough numbers; the calculator will give precise ones).
   * Explore the breakdown: you can expand each service to see details (e.g., EC2 cost = instance cost + EBS cost + data transfer; RDS cost = instance hours + storage; S3 cost = storage + request + data transfer).
7. **Adjust and Experiment (Optional):** Try changing some parameters to see cost impact:

   * For example, switch the EC2 instance from t2.micro to t3.large and update usage – watch the EC2 cost jump significantly (reflecting a more powerful instance).
   * Or change RDS to Multi-AZ and see the cost roughly double.
   * This helps you understand which components drive cost.
8. **Save or Note the Estimate:** If logged in, you can save the estimate. If not, you can still see a **shareable link** or export as PDF. It’s not necessary for the lab, but good to know.
9. **No Cleanup Needed:** The Pricing Calculator doesn’t create resources, so no AWS resources to delete. Just close the browser when done.

**End Result:** You have used the AWS Pricing Calculator to estimate cloud costs for a simple application stack. You now know how to add services to an estimate and interpret the cost report. This skill directly translates to exam scenarios where you might be asked how to get pricing information or compare cost options. You saw how on-demand EC2 is priced by hourly usage, how storage and data transfer incur additional costs, and how a managed service like RDS might be priced monthly.

> *Note:* The **AWS Pricing Calculator** is an official tool to **estimate costs**. The CCP exam may not require you to produce exact numbers, but you should know *what* affects cost (for example, running time of instances, data egress, storage size, etc.) and that this calculator exists to help plan expenses. Also remember, AWS offers the **Free Tier** for 12 months for new accounts (e.g., 750 hours of t2.micro, certain amount of S3 and RDS usage free). In our estimate, we effectively priced beyond free tier usage. In a real scenario, always consider if your workload fits in free tier to save money. Finally, know that there are other cost management tools like **AWS Cost Explorer** and **AWS Budgets** (not covered in this lab) which help you monitor and control ongoing costs. For now, you have hands-on familiarity with the pricing calculator – a practical aid for AWS planning and the exam.

---

## Conclusion and Exam Alignment

By completing these labs, you've gained practical experience across all the key knowledge domains of the AWS Certified Cloud Practitioner exam. We covered **Identity and Security** (IAM user management), **Compute** (EC2 instances, Lambda functions), **Storage** (S3 bucket usage and static websites), **Networking** (VPC basics with subnets and IGWs), **Databases** (RDS instance setup), **Monitoring** (CloudWatch alarms and logs), **Infrastructure as Code** (CloudFormation template deployment), and **Billing** (using the Pricing Calculator). These hands-on exercises mirror real-world tasks and solidify the concepts that appear in exam questions.

Remember that the CCP exam is about understanding AWS at a fundamental level. Through these labs, you have *experienced* those fundamentals:

* Launching and configuring resources in AWS and observing how they function and interconnect.
* Applying the principle of least privilege with IAM.
* Seeing the difference between managed services (like RDS) and unmanaged (EC2 DIY setup).
* Monitoring resource utilization and understanding AWS's global infrastructure (regions/AZs) when setting up networking.
* Estimating costs and recognizing what drives AWS pricing.

According to AWS instructors, experiencing the cloud first-hand helps cement your knowledge. Hands-on practice is often the difference between simply memorizing facts and truly understanding AWS concepts. By following this lab guide, you have built up that practical knowledge. These labs were sourced from community experts and tailored to align with the CCP objectives, so you can be confident that you're practicing the right skills in the right way.

As a final recommendation, always clean up your AWS resources after practice. Not only does this prevent unwanted charges (as emphasized in our labs), but it also gives you practice in resource lifecycle management – an often overlooked aspect of cloud proficiency.

Happy cloud learning, and good luck on your AWS Certified Cloud Practitioner exam! With the hands-on experience gained here, you're well on your way to scoring full marks and becoming a confident AWS Cloud Practitioner. **Enjoy your cloud journey** 🚀.
