# AWS Certified Cloud Practitioner 4-Week Study Plan

## WEEK 1: Cloud Concepts (24%)

### 1.1 Define the Benefits of the AWS Cloud
- Explain the six advantages of cloud computing.
- Understand the value proposition of AWS (agility, elasticity, flexibility).
- Recognize the benefits of scalability, speed of deployment, and global reach.

### 1.2 Identify Design Principles of the AWS Cloud
- Understand the AWS Well-Architected Framework.
- Know its six pillars:
  - Operational Excellence
  - Security
  - Reliability
  - Performance Efficiency
  - Cost Optimization
  - Sustainability
- Differentiate between the design pillars and when to apply them.

### 1.3 Understand Benefits and Strategies for Migration to AWS Cloud
- Identify business benefits of migration (reduced risk, increased agility).
- Understand AWS Cloud Adoption Framework (AWS CAF).
- Identify the six AWS CAF perspectives: Business, People, Governance, Platform, Security, Operations.
- Recognize appropriate migration strategies (rehosting, replatforming, repurchasing, refactoring, retiring, retaining).
- Understand tools and services used in migration (AWS Migration Hub, Snowball, Database Migration Service).

### 1.4 Understand Concepts of Cloud Economics
- Compare CapEx (on-premises) vs OpEx (cloud).
- Understand variable vs fixed costs.
- Grasp the concept of rightsizing.
- Identify cost benefits of managed services (like RDS vs self-managed DB).
- Learn automation benefits: provisioning, configuration via AWS CloudFormation.
- Understand service pricing considerations (e.g., data transfer, compute hours).

## WEEK 2: Security and Compliance (30%)

### 2.1 Understand the AWS Shared Responsibility Model
- Distinguish between customer responsibility and AWS responsibility.
- Know how it varies by service type (IaaS vs PaaS vs SaaS).
- Understand AWS-managed vs customer-managed aspects (OS patching, network config, encryption, data protection).

### 2.2 Understand AWS Cloud Security, Governance, and Compliance Concepts
- Define AWS security tools: AWS Security Hub, Inspector, GuardDuty.
- Understand data encryption at rest and in transit.
- Know compliance frameworks AWS supports (HIPAA, PCI DSS, GDPR).
- Use AWS Artifact to access audit reports.
- Understand logging and monitoring: AWS CloudTrail, CloudWatch, AWS Config.

### 2.3 Identify AWS Access Management Capabilities
- Understand the purpose and function of IAM: users, groups, roles, policies.
- Know how to apply least privilege principle.
- Recognize IAM best practices (e.g., using roles for services, avoiding root usage).
- Understand multi-factor authentication (MFA) and password policies.
- Explore AWS IAM Identity Center (AWS SSO) and federated access.
- Use tools like AWS Trusted Advisor for security checks.
- Recognize sources of security information (Security Blog, Security Center).

## WEEK 3: Cloud Technology and Services (34%)

### 3.1 Define Methods of Deploying and Operating in the AWS Cloud
- Use the AWS Management Console, AWS CLI, SDKs, CloudShell.
- Understand Infrastructure as Code via AWS CloudFormation, AWS CDK.
- Describe different operating models: serverless, containers, VMs.

### 3.2 Define the AWS Global Infrastructure
- Understand Regions, Availability Zones, Edge Locations, Local Zones, Wavelength Zones.
- Know which services are regional or global.
- Understand how infrastructure choices affect availability, latency, and compliance.

### 3.3 Identify AWS Compute Services
- Amazon EC2: virtual machine-based compute
- AWS Lambda: event-driven serverless
- Amazon ECS/EKS: container orchestration
- AWS Elastic Beanstalk: platform-as-a-service deployment
- Understand when to use which service

### 3.4 Identify AWS Database Services
- Amazon RDS: managed relational DBs (MySQL, PostgreSQL, Oracle, etc.)
- Amazon DynamoDB: managed NoSQL
- Amazon Redshift: analytics/data warehousing
- Amazon ElastiCache: in-memory caching
- Amazon Neptune: graph database
- Choose appropriate DB for given use cases

### 3.5 Identify AWS Network Services
- Amazon VPC: Virtual private cloud
- Subnets (public/private), route tables
- Security groups vs Network ACLs
- Internet Gateway, NAT Gateway
- Elastic Load Balancing, Elastic IPs
- Basic routing and isolation concepts

### 3.6 Identify AWS Storage Services
- Amazon S3: object storage
- Amazon EBS: block storage
- Amazon EFS: shared file storage
- Amazon Glacier: archival
- Data lifecycle policies, S3 storage classes (Standard, IA, Glacier, One Zone)

### 3.7 Identify AWS AI/ML and Analytics Services
- Amazon SageMaker: build, train, deploy ML
- Amazon Rekognition: image/video analysis
- Amazon Comprehend: NLP/text analysis
- Amazon Kinesis, AWS Glue, Athena, QuickSight for real-time and batch analytics

### 3.8 Identify Services from Other AWS Categories
- Amazon CloudFront: CDN
- Amazon SNS / SQS: messaging services
- AWS Step Functions: workflow orchestration
- Amazon CloudWatch: metrics/logs
- AWS CloudTrail: API tracking
- AWS Config: compliance tracking
- AWS Organizations: consolidated billing/account structure
- AWS Trusted Advisor: checks for best practices

## WEEK 4: Billing, Pricing, and Support (12%)

### 4.1 Compare AWS Pricing Models
- Understand On-Demand, Reserved, and Spot Instances
- Compare with Savings Plans
- Explore billing for different services (e.g., per second, per GB)

### 4.2 Understand AWS Billing and Cost Management Resources
- AWS Budgets
- AWS Cost Explorer
- AWS Pricing Calculator
- Cost allocation tags and reports
- Use of consolidated billing in AWS Organizations

### 4.3 Identify AWS Support Options and Technical Resources
- AWS Support Plans:
  - Basic (free), Developer, Business, Enterprise
- Compare features like TAMs, 24/7 support, response SLAs
- Use AWS Knowledge Center, whitepapers, FAQs, documentation
