# AWS Certified Solutions Architect - Associate

## The Exam

### Domain 1: Design Resilient Architectures

1.1 Design reliable/resilient storage
1.2 Determine how to design decoupling mechanisms using AWS services
1.3 Determine how to design a multi-tier architecture solution
1.4 Determine how to design high availability and/or fault tolerant solutions

EC2 Instance Store

* Ephemeral volumes
* Only certain EC2 instances
* Fixed capacity
* Disk type and capacity depends on EC2 instance type
* Application-level durability

Elastic Block Store

* Different types
* Encryption
* Snapshots
* Provisioned capacity
* Independent lifecycle than EC2 instance
* Multiple volumes striped to create larger volumes

Volume Type: General Purpose SSD (gp2)
Use Cases:

* Recommended for most workloads
* System boot volumes
* Virtual desktops
* Low-latency interactive apps
* Dev and test environments

Volume Size: 1 GiB - 16 TiB
Max. IOPS/Volume: 10,000
Max. Throughput/Volume: 160 MiB/s
Dominant Performance Attribute: IOPS

Volume Type: Provisioned IOPS SSD (io1)
Use Cases:

* Critical business applications that require sustained IOPS performance, or more than 10,000 IOPS or 160 MiB/s of throughput per volume
* Large database workloads

Volume Size: 4 GiB - 16 TiB
Max. IOPS/Volume: 32,000
Max. Throughput/Volume: 500 MiB/s
Dominant Performance Attribute: IOPS

Volume Type: Throughput Optimized HDD (st1)
Use Cases:

* Streaming workloads requiring consistent, fast throughput at a low price
* Big data
* Data warehouses
* Log processing
* Cannot be a boot volume

Volume Size: 500 Gib - 16 TiB
Max. IOPS/Volume: 500
Max. Throughput/Volume: 500 MiB/s
Dominant Performance Attribute: MiB/s

Volume Type: Cold HDD (sc1)
Use Cases:

* Throughput-oriented storage for large volumes of data that is infrequently accessed
* Scenarios where the lowest storage cost is important
* Cannot be a boot volume

Volume Size: 500 GiB - 16 TiB
Max. IOPS/Volume: 250
Max. Throughput/Volume: 250 MiB/s
Dominant Performance Attribute: MiB/s

Where to Fill Your Gaps

* FAQ - EBS
* Whitepaper - AWS Storage Services Overview
* Lab - Introduction to Amazon Elastic Block Storage (EBS)
* Amazon EC2 Instance Store

Amazon EFS

* File storage in the AWS Cloud
* Shared storage
* Petabyte-scale file system
* Elastic capacity
* Supports NFS v4.0 and 4.1 (NFSv4) protocol
* Compatible with Linux-based AMIs for Amazon EC2

Amazon S3

* Consistency model
* Storage classes & Durability - Standard, Standard-IA
* Encryption (data at rest) - SSE-S3, SSE-KMS, SSE-C
* Encryption (data at transit) - HTTPS
* Versioning
* Access control
* Multi-part upload
* Internet-API accessible
* Virtual unlimited capacity
* Regional Availability
* Highly durable - 99.999999999%

Amazon Glacier

* Data backup and archive storage
* Vaults and archives
* Retrievals - expedited, standard, bulk
* Encryption
* Amazon S3 object lifecycle policy
* Regional availability
* Highly durable - 99.999999999%

Where to Fill Your Gaps

* Amazon Glacier Documentation
* FAQs - EFS, S3, Glacier, CloudFront
* Lab - Introduction to Amazon Simple Storage Service (S3)
* Lab - Introduction to Amazon Elastic File System (EFS)

Loosely Coupled Architectures
SQS
ELB
Elastic IP Addresses!

Where to Fill Your Gaps

* Study Guide Chapter 8
* SQS FAQs
* ELB FAQs
* ELB Documentation
* EIP FAQs
* Route 53 Documentation

Fault Tolerance
The more loosely your system is coupled, the more easily it scales and the more fault-tolerant it can be.

Where to Fill Your Gaps

* Individual service FAQs
* AWS Well Architected Framework

CloudFormation

* Declarative programming language for deploying AWS resources
* Uses templates and stacks to provision resources
* Create, update, and delete a set of resources as a single unit (stack)

Template:

* Basic definition of resources to create
* JSON text file

Stack:

* Collection of resources

Mappings, parameters, AMI IDs are different across regions, best way to specify them is through mappings, parameters are used for user input (!!!)

AWS Lambda

* Fully managed compute service that runs stateless code (Node.js, Java, C#, Go and Python) in response to an event or on a time-based interval
* Allows you to run code without managing infrastructure like Amazon EC2 instances and Auto Scaling groups

Test Axioms

* Expect "Single AZ" will never be a right answer
* Using AWS managed services should always be preferred
* Fault tolerant and high availability are not the same thing
* Expect that everything will fail at some point and design accordingly

### Domain 2: Design Performant Architectures

2.1 Choose performant storage and databases
2.2 Apply caching to improve performance
2.3 Design solutions for elasticity and scalability

Amazon S3 Buckets

To upload your data (photos, videos, documents):

1. Create a bucket in one of the AWS Regions
2. Upload any number of objects to the bucket

Bucket: Virtual hosted-based URLs

* http://bucket.s3.amazonaws.com
* http://bucket.s3-aws-region.amazonaws.com

Object: object name as key

* https://s3-ap-northeast-1.amazonaws.com/[bucket name]/*Preview2.mp4*

Amazon S3: Payment Model

Pay only for what you use

* GBs per month
* Transfer out of region
* PUT, COPY, POST, LIST, and GET requests

Free of charge

* Transfer in to Amazon S3
* Transfer out from Amazon S3 to Amazon CloudFront or the same region

Amazon S3: Storage Classes

General purpose: Amazon S3 Standard

* Higher availability requirements: Use cross-region replication

Infrequently accessed data: Amazon S3 Standard - Infrequent Access

* Lower cost per GB stored
* Higher cost per PUT, COPY, POST, or GET requests
* 30-day storage minimum

Lifecycle Policies

Amazon S3 lifecycle policies allow you to delete or move objects based on age.

Amazon S3 Standard > (30 days) > Amazon S3 Standard - IA > (60 days) > Amazon Glacier > (365 days) > Delete

Amazon Databases

* Amazon Relational Database Service (RDS)
* Amazon DynamoDB
* Amazon Redshift

When to Use Amazon RDS

Use Amazon RDS

* Complex transactions or complex queries
* A medium-to-high query/write rate
* No more than a single worker node/shard
* High durability

Do not use Amazon RDS

* Massive read/write rates (e.g. 150K write/second)
* Sharding
* Simple GET/PUT requests and queries
* RDBMS customization

DynamoDB: Provisioned Throughput

Allocates resources based on throughput capacity requirements (read/write)

Read capacity unit (for an item up to 4 KB in size): **RCU**

* One strongly consistent read per second
* Two eventually consistent reads per second

Write capacity unit (for an item up to 4 KB in size): **WCU**

* One write per second

Applying Caching to Improve Performance

* CloudFront
* ElastiCache

Memcached vs. Redis

Memcached

* Multithreading
* Low maintenance
* Easy horizontal scalability with Auto Discovery

Redis

* Support for data structures
* Persistence
* Atomic operations
* Pub/Sub messaging
* Read replicas/failover
* Cluster mode/sharded clusters

Amazon CloudFront

* Use case and benefits
* Content - static and dynamic
* Origins - S3, EC2, ELB, HTTP servers
* Protect private content
* Improve security: AWS Shield Standard and Advanced, AWS WAF

Vertical Scaling vs. Horizontal Scaling

* Vertical scaling (scale up and down): Change in the specifications of instances (more CPU, memory)
* Horizontal scaling (scale in and out): Change in the number of instances (add and remove instances as needed)

Auto Scaling

* Launches or terminates instances
* Automatically registers new instances with load balancers
* Can launch across Availability Zones
* Launch configuration: contains AMI ID, instance type, key pair (?), and user data (initial script?)

Auto Scaling Components

Auto Scaling launch configuration

* Specifies EC2 instance size and AMI name

Auto Scaling group

* References the launch configuration
* Specifies min, max, and desired size of the Auto Scaling group
* May reference an ELB
* Health Check Type

Auto Scaling Policy

* Specifies how much to scale in or scale out
* One or more may be attached to Auto Scaling group

Test Axioms

* If data is unstructured, Amazon S3 is generally the storage solution
* Use caching strategically to improve performance
* Know when and why to use Auto Scaling
* Choose the instance and database type that makes the most sense for your workload and performance need

### Domain 3: Specify Secure Applications and Architectures

3.1 Determine how to secure application tiers
3.2 Determine how to secure data
3.3 Define the networking infrastructure for a single VPC application

Infrastructure

Infrastructure

* Shared responsibility model

Protecting your AWS resources

* Principle of least privilege
* Identities

Principle of Least Privilege

* Persons (or processes) can perform all activities they need to perform, and no more

AWS IAM

* Centrally manage users and user permissions in AWS
* Using AWS IAM, you can create users, groups, roles, and policies, and define permissions to control which AWS resources users can access
* IAM integrates with Microsoft Active Directory and AWS Directory Service using SAML identity federation

Identities

* IAM users: Users created within the account
* Roles: Temporary identities used by EC2 instances, Lambdas, and external users
* Federation: Users with Active Directory identities or other corporate credentials have role assigned in IAM
* Web Identity Federation: Users with web identities from Amazon.com or other Open ID provider have role assigned using Security Token Service (STS)

Fill Your Knowledge Gap

* Overview of Security Processes Whitepaper: Shared Security Responsibility Model, AWS Account Security Features
* AWS Security Best Practices Whitepaper
* IAM Best Practices
* When to use AWS STS

Compute/Network Architecture

Design your network architecture in the cloud

* Security
* Routing
* Network isolation
* Management
* Bastion hosts

Virtual Private Cloud

* Organization: Subnets
* Security: Security groups/access control lists
* Network isolation: Internet gateways/virtual private gateways/NAT gateways
* Traffic direction: routes

How to Use Subnets

Recommendation: Use subnets to define internet accessibility

Public Subnets

* To support inbound/outbound access to the public internet, include a routing table entry to an **internet gateway**

Private Subnets

* Do not have a routing table entry to an internet gateway
* Not directly accessible from the public internet
* To support restricted, outbound-only public internet access, typically use a "jump box" (NAT/proxy/bastion host)

Security Groups vs. Network ACLs

             | Security Groups                     | Access Control Lists
-------------|-------------------------------------|------------------------------------
Access Type  | Specify a port, protocol, source IP | Specify a port, protocol, source IP
Rules        | Explicit Allow only                 | Explicit Allow or Deny
State        | Stateful                            | Stateless
Application  | Applied to ENIs                     | Applied to subnets
Association  | Associated with a single VPC        | Associated with single VPC
Supports     | VPC and EC2 Classic                 | VPC only

Security Groups

Use security groups to control traffic into, out of, and between resources

VPC Connections

Know the services to get traffic in or out of your VPC

* Internet gateway: Connect to the internet
* Virtual private gateway: Connect to VPN
* AWS Direct Connect: Dedicated pipe
* VPC peering: Connect to other VPCs
* NAT gateways: Allow internet traffic from private subnets

Fill Your Knowledge Gap

* VPC Fundamentals video
* Bastion Hosts
* VPC User Guide

Data Tier

Data in transit

* In and out of AWS
* Within AWS

Data at rest

* Amazon S3
* Amazon EBS

Data in Transit

Transferring data in and out of your AWS infrastructure

* SSL over web
* VPN for IPSec
* IPSec over AWS Direct Connect
* Import/Export/Snowball

Data sent to the AWS API

* AWS API calls use HTTPS/SSL by default

Data at Rest

Data stored in Amazon S3 is private by default, requires AWS credentials for access

* Access over HTTP or HTTPS
* Audit of access to all objects
* Supports ACL and policies: buckets, prefixes (directory/folder), objects

Server-side encryption options

* Amazon S3-Managed Keys (SSE-SE)
* KMS-Managed Keys (SSE-KMS)
* Customer-Provided Keys (SSE-C)

Client-side encryption options

* KMS managed master encryption keys (CSE-KMS)
* Customer managed master encryption keys (CSE-C)

Managing Your Keys

Key Management Service

* Customer software-based key management
* Integrated with many AWS services
* Use directly from application

AWS CloudHSM

* Hardware-based key management
* Use directly from application
* FIPS 140-2 compliance

Integrating AWS KMS

Services integrated with AWS KMS

* Amazon EBS
* Amazon S3
* Amazon RDS
* Amazon Redshift
* Amazon Elastic Transcoder
* Amazon WorkMail
* Amazon EMR

Where to Fill Your Gaps

* Protecting Your Data in AWS (video)
* Encrypting Data at Rest (whitepaper)
* Encryption in Amazon S3
* Security Overview (whitepaper)
* IAM Policies and Bucket Policies and ACLs!

Test Axioms

* Lock down the root user
* Security groups only allow. Network ACLs allow explicit deny.
* Prefer IAM roles to access keys

### Domain 4: Design Cost-Optimized Architectures

4.1 Determine how to design cost-optimized storage
4.2 Determine how to design cost-optimized compute

AWS Pricing Overview

* Pay as you go
* Pay less when you reserve
* Pay even less per unit by using more

Fundamental Pricing Characteristics

Three fundamental characteristics you pay for with AWS:

* Compute
* Storage
* Data Transfer

Amazon EC2 Pricing

Considerations for estimating the cost of using Amazon EC2:

1. Clock hours of server time
2. Machine configuration
3. Machine purchase type
4. Number of instances
5. Load balancing
6. Detailed monitoring
7. Auto Scaling
8. Elastic IP Addresses
9. Operating systems and software packages

Amazon EC2 Pricing Factors

* EC2 Instance Type
* Tenancy
* Pricing options

Instance storage is free but ir ephemeral

Amazon EC2: Ways to Save Money

Reserved Instances

EC2 Reserved Instances (RI) provide a significant discount (up tp 75%) compared to on-demand pricing.

RI Types

* Standard RIs
* Convertible RIs
* Scheduled RIs

Spot Instances

Spot Instances are spare compute capacity in the AWS Cloud available to you at steep discounts compared to on-demand prices (30 to 45%)

Amazon S3 Pricing

Considerations for estimating the cost of using Amazon EC2:

1. Storage class: Standard Storage, Standard - Infrequent Access, Glacier
2. Storage
3. Requests
4. Data transfer

Amazon EBS Pricing

Considerations for estimating the cost of using Amazon EBS:

1. Volumes
2. Input/output operations per second (IOPS)
3. Snapshots
4. Data transfers

Solid State Drives (SSDs) vs. Hard Disk Drives (HDDs)

Serverless Architectures

Recognize the opportunity to reduce compute spend through serverless architectures

* AWS Lambda
* Amazon S3
* Amazon DynamoDB
* Amazon API Gateway

Storage: Amazon CloudFront

Use cases:

* Content - Static and dynamic
* Origins - Amazon S3, EC2, Elastic Load Balancing, HTTP servers

Cost benefits:

* No cost for data transfer between S3 and CloudFront
* Can be used to reduce the compute workload for EC2 instances

Caching with CloudFront

Considerations for estimating the cost of using Amazon CloudFront:

1. Traffic distribution
2. Requests
3. Data transfer out

Caching with CloudFront can have positive impacts on both performance and cost-optimization!

Test Axioms

* If you know it's going to be on, reserve it
* Any unused CPU time is a waste of money
* Use the most cost-effective data storage services and class
* Determine the most cost-effective EC2 pricing model and instance type for each workload

### Domain 5: Define Operationally-Excellent Architectures

5.1 Choose design features in solutions that enable operational excellence

Operational Excellence

The ability to run and monitor systems to deliver business value and continually improve supporting processes and procedures.

* Prepare
* Operate
* Evolve

Operational Excellence: Design Principles

* Perform operations with code
* Annotate documentation
* Make frequent, small, reversible changes
* Refine operations procedures frequently
* Anticipate failure
* Learn from all operational failures

AWS Services Supporting Operational Excellence

* AWS Config
* AWS CloudFormation
* AWS Trusted Advisor
* AWS Inspector
* VPC Flow Logs
* AWS CloudTrail

CloudWatch

Test Axioms

* IAM roles are easier and safer than keys and passwords
* Monitor metrics across the system
* Automate responses to metrics where appropriate
* Provide alerts for anomalous conditions

## Identity Access Management & S3

### What is IAM?

IAM allows you to manage users and their level of access to the AWS Console.

It is important to understand IAM and how it works, both for the exam and for administrating a company's AWS account in real life.

### Key Features of IAM

Identity Access Management (IAM) offers the following features:

* Centralised control of your AWS account
* Shared access to your AWS account
* Granular permissions
* Identity Federation (including Active Directory, Facebook, LinkedIn, etc.)
* Multifactor Authentication
* Provide temporary access for users/devices and services where necessary
* Allows you to set up your own password rotation policy
* Integrates with many different AWS services
* Supports PCI DSS Compliance

### Key terminology for IAM

1. **Users**. End Users such as people, employees of an organization, etc.
2. **Groups**. A collection of users. Each user in the group will inherit the permissions of the group.
3. **Policies**. Policies are made up of documents. These documents are in a format called JSON and they give permissions as to what a User/Group/Role is able to do.
4. **Roles**. You can create roles and then assign them to AWS resources.

### Exam Tips

* IAM is universal. It does not apply to regions at this time.
* The "root account" is simply the account created when first setup your AWS account. It has complete Admin access.
* New users have **no permissions** when first created.
* New users are assigned **Access Key ID** & **Secret Access Keys** when first created.
* These are not the same as a password. You cannot use the access key ID and secret access key to login to the console. You can use this to access AWS via the APIs and the command line, however.
* You only get to view these once. If you lose them, you have to regenerate them. So, save them in a secure location.
* Always set Multifactor Authentication on your root account.
* You can create and customise your own password rotation policies.

### What is S3?

S3 provides developers and IT teams with secure, durable, highly-scalable object storage. Amazon S3 is easy to use, with a simple web services interface to store and retrieve any amount of data from anywhere on the web.

* S3 is a safe place to store your files.
* It is object-based storage.
* The data is spread across multiple devices and facilities.

The basics of S3 are as follows:

* S3 is **object-based** - i.e., allows you to upload files.
* Files can be from 0 bytes to 5 TB.
* There is unlimited storage.
* Files are stored in **buckets**.
* **S3 is a universal namespace**. That is, names must be unique globally.
* https://s3-eu-west-1.amazonaws.com/acloudguru
* When you upload a file to S3, you will receive a **HTTP 200 code** if the upload was successful.

S3 is is object based. Think of objects just as files. Objects consist of the following:

* Key - this is simply the name of the object
* Value - this is simply the data and is made up of a sequence of bytes
* Version ID - important for versioning
* Metadata - data about data you are storing
* Subresources - access control lists, torrent

#### Data Consistency Model for S3

* Read after write consistency for PUTs of new objects
* Eventual consistency for overwrite PUTs and DELETEs (can take some time to propagate)

In other words,

* If you write a new file and read it immediately afterwards, you will be able to view that data.
* If you update an existing file or delete a file and read it immediately, you may get the older version, or you may not. Basically, changes to objects can take a little bit of time to propagate.

S3 has the following guarantees from Amazon:

* Built for 99.99% availability for the S3 platform.
* Amazon guarantees 99.9% availability.
* Amazon guarantees 99.999999999% durability for S3 information (remember 11 x 9s).

S3 has the following features:

* Tiered storage available
* Lifecycle management
* Versioning
* Encryption
* MFA delete
* Secure your data using **Access Control Lists** and **Bucket Policies**.

#### S3 Storage Classes

1. **S3 Standard**. 99.99% availability, 99.999999999% durability, stored redundantly across multiple devices in multiple facilities, and is designed to sustain the loss of 2 facilities concurrently.
2. **S3 - IA** (Infrequently Accessed). For data that is accessed less frequently, but requires rapid access when needed. Lower fee than S3, but you are charged a retrieval fee.
3. **S3 One Zone - IA**. For when you want a lower-cost option for infrequently accessed data, but do not require the multiple Availability Zone data resilience.
4. **S3 - Intelligent Tiering**. Designed to optimize costs by automatically moving data to the most cost-effective access tier, without performance impact or operational overhead.
5. **S3 Glacier**. S3 Glacier is a secure, durable, and low-cost storage class for data archiving. You can reliably store any amount of data at costs that are competitive with or cheaper than on-premises solutions. Retrieval times configurable from minutes to hours.
6. **S3 Glacier Deep Archive**. S3 Glacier Deep Archive is Amazon S3's lowest-cost storage class, where a retrieval time of 12 hours is acceptable.

#### S3 - Charges

You are charged for S3 in the following ways:

* Storage
* Requests
* Storage management pricing
* Data transfer pricing
* Transfer acceleration
* Cross region replication pricing

#### S3 Transfer Acceleration

Amazon S3 Transfer Acceleration enables fast, easy, and secure transfers of files over long distances between your end users and an S3 bucket.

Transfer Acceleration takes advantage of Amazon CloudFront's globally distributed edge locations. As the data arrives at an edge location, data is routed to Amazon S3 over an optimized network path.

#### Tips

* Control access to buckets using either a **bucket ACL** or using **bucket policies**.

### S3 Security and Encryption

By default, all newly created buckets are **private**. You can set up access control to your buckets using:

* Bucket Policies
* Access Control Lists

S3 buckets can be configured to create access logs which log all requests made to the S3 bucket. This can be sent to another bucket and even another bucket in another account.

#### Encryption

Encryption in Transit is achieved by

* SSL/TLS

Encryption at Rest (Server Side) is achieved by

* S3 Managed Keys - SSE-S3
* AWS Key Management Service, Managed Keys - SSE-KMS
* Server Side Encryption with Customer Provided Keys - SSE-C

Client Side Encryption

#### S3 Versioning

Using versioning with S3:

* Stores all versions of an object (including all writes and even if you delete an object)
* Great backup tool
* Once enabled, **versioning cannot be disabled**, only suspended
* Integrates with **Lifecycle** rules
* Versioning's **MFA Delete** capability, which uses multi-factor authentication, can be used to provide an additional layer of security.

#### Lifecycle Management with S3

* Automates moving your objects between the different storage tiers
* Can be used in conjunction with versioning
* Can be applied to current versions and previous versions

#### Cross Region Replication

* Versioning must be enabled on both the source and destination buckets.
* Regions must be unique.
* Files in an existing bucket are not replicated automatically.
* All subsequent updated files will be replicated automatically.
* Delete markers are not replicated.
* Deleting individual versions or delete markers will not be replicated.

#### S3 Transfer Acceleration

S3 Transfer Acceleration utilises the CloudFront Edge Network to accelerate your uploads to S3. Instead of uploading directly to your S3 bucket, you can use a distinct URL to upload directly to an edge location which will then transfer that file to S3. You will get a distinct URL to upload to (acloudguru.s3-accelerate.amazonaws.com)

### CloudFront

A content delivery network (CDN) is a system of distributed servers (network) that deliver webpages and other web content to a user based on the geographic locations of the user, the origin of the webpage, and a content delivery server.

#### Key Terminology

* Edge Location - This is the location where content will be cached. This is separate to an AWS region / AZ.
* Origin - This is the origin of all the files that the CDN will distribute. This can be an S3 bucket, an EC2 instance, an Elastic Load Balancer, or Route53.
* Distribution - This is the name given to the CDN which consists of a collection of Edge Locations.

Amazon CloudFront can be used to deliver your entire website, including dynamic, static, streaming, and interactive content using a global network of edge locations. Requests for your content are automatically routed to the nearest edge location, so content is delivered with the best possible performance.

* Web Distribution - typically used for websites
* RTMP - used for media streaming

#### Exam Tips

* Edge locations are not just read only - you can write to them too (i.e., put an object on to them.)
* Objects are cached for the life of the **TTL** (**Time To Live**.)
* You can clear cached objects, but you will be charged.

## EC2

## Databases on AWS

AWS Database types

### RDS (OLTP)

* SQL
* MySQL
* PostgreSQL
* Oracle
* Aurora
* MariaDB

DynamoDB (No SQL)
Red Shift OLAP

Elasticache
* Memcached
* Redis

Relational Databases

* RDS runs on virtual machines
* You cannot log in to these operating systems however.
* Patching of the RDS Operating System and DB is Amazon's responsibility
* RDS is NOT Serverless
* Aurora Serverless IS Serverless

There are two different types of backups for RDS:

* Automated backups
* Database snapshots

Read replicas

* Can be Multi-AZ
* Used to increase performance
* Must have backups turned on
* Can be in different regios
* Can be MySQL, PostgreSQL, MariaDB, Oracle, Aurora (not SQL Server)
* Can be promoted to master, this will break the read replica

MultiAZ

* Used for DR
* You can force a failover from one AZ to another by rebooting the RDS instance

Encryption at rest is supported for MySQL, Oracle, SQL Server, PostgreSQL, MariaDB & Aurora. Encryption is done using the AWS Key Management Service (KMS) service. Once your RDS instance is encrypted, the data stored at rest in the underlying storage is encrypted, as are its automated backups, read replicas, and snapshots.

### DynamoDB

DynamoDB is the AWS managed NoSQL database service. It has many features that are being added to constantly, making it a great service to use for many different requirements. The feature which was incorrect is DynamoDB only being single availability zone by default making this the correct answer. DynamoDB is distributed across three geographically distinct datacentres by default, all of the other options listed are valid features of DynamoDB. Further information: https://aws.amazon.com/dynamodb/faqs/

* Stored on SSD storage
* Spread across 3 geographically distinct datacenters
* Eventual Consistent Reads (default)
* Strongly Consistent Reads

### Redshift

* Used for business intelligence
* Available in only 1 AZ

Redshift backups

* Enabled by default with a 1 day retention period
* Maximum retention period is 35 days
* Redshift always attempts to maintain at least three copies of your data (the original and replica on the compute nodes and a backup in Amazon S3)
* Redshift can also asynchronously replicate your snapshots to S3 in another region for disaster recovery

### Aurora

Amazon Aurora is a MySQL and PostgreSQL-compatible relational database engine that combines the speed and availability of high-end commercial databases with the simplicity and cost-effectiveness of open source databases. Amazon Aurora provides up to five times better performance than MySQL and three times better than PostgreSQL databases at a much lower price point, whilst delivering similar performance and availability.

Things to know about Aurora:

* Start with 10 GB, scales in 10 GB increments to 64 TB (storage autoscaling)
* Compute resources can scale up to 32 vCPUs and 244 GB of memory
* 2 copies of your data is contained in each availability zone, with minimum of 3 availability zones. 6 copies of your data.

Scaling Aurora

* Aurora is designed to transparently handle the loss of up to two copies of data without affecting database write availability and up to three copies without affecting read availability
* Aurora storage is also self-healing: data blocks and disks are continuously scanned for errors and repaired automatically

Two types of Aurora replicas:

* Aurora replicas (currently 15)
* MySQL read replicas (currently 5)

Feature                                             Amazon Aurora replicas          MySQL replicas
Number of replicas                                  Up to 15                        Up to 5
Replication type                                    Asynchronous (milliseconds)     Asynchronous (seconds)
Performance impact on primary                       Low                             High
Act as failover target                              Yes (no data loss)              Yes (potentially minutes of data loss)
Automated failover                                  Yes                             No
Support for user-defined replication delay          No                              Yes
Support for different data or schema vs. primary    No                              Yes

Backups with Aurora

* Automated backups are always enabled on Amazon Aurora DB instances. Backups do not impact database performance.
* You can also take snapshots with Aurora. This also does not impact performance.
* You can share Aurora Snapshots with other AWS accounts.

### Elasticache

ElastiCache is a web service that makes it easy to deploy, operate, and scale an in-memory cache in the cloud. The service improves the performance of web applications by allowing you to retrieve information from fast, managed, in-memory caches, instead of relying entirely on slower disk-based databases.

ElastiCache supports two open-source in-memory caching engines: Memcached and redis.

Requirement                     Memcached   Redis
Simple cache to offload DB      Yes         Yes
Ability to scale horizontally   Yes         Yes
Multi-threaded performance      Yes         No
Advanced data types             No          Yes
Ranking/Sorting data sets       No          Yes
Pub/Sub capabilities            No          Yes
Persistence                     No          Yes
Multi-AZ                        No          Yes
Backup & restore capabilities   No          Yes

* Use Elasticache to increase database and web application performance.
* Redis is Multi-AZ
* You can do backups and restores of Redis
* If you need to scale horizontally, use Memcached

### Quiz

* Amazon Athena is an interactive query service that makes it easy to analyze data in Amazon S3, using standard SQL commands. It will work with a number of data formats including JSON, Apache Parquet, Apache ORC among others (https://aws.amazon.com/athena/faqs/)
* Provisioned IOPS becomes important when you are running production environments requiring rapid responses, such as those which run e-commerce websites. Without high-performant responses from an RDS instance, page loads of the website could suffer resulting in loss of business. If your workloasds are not latency sensitive or you are running a test environment, the additional cost of provisioned IOPS will not be cost beneficial to your project. (https://aws.amazon.com/ebs/features/)
* There will always be a charge for provisioning read and write capacity and the storage of data within DynamoDB, therefore these two answers are correct. There is no charge for the transfer of data into DynamoDB, providing you stay within a single region (if you cross regions, you will be charged at both ends of the transfer.) There is no charge for the actual number of tables you can create in DynamoDB, providing the RCU and WCU are set to 0, however in practice you cannot set this to anything less than 1 so there always be a nominal fee associated with each table. Further information: https://aws.amazon.com/dynamodb/pricing/

## Route53

Route53 Exam Tips

* ELBs do not have pre-defined IPv4 addresses; you resolve to them using a DNS name.
* Understand the difference between an Alias record and a CNAME.
* Given the choice, always choose an alias record over a CNAME.

Alias Records have special functions that are not present in other DNS servers. Their main function is to provide special functionality and integration into AWS services. Unlike CNAME records, they can also be used at the Zone Apex, where CNAME records cannot. Alias Records can also point to AWS Resources that are hosted in other accounts by manually entering the ARN.

Common DNS Types

* SOA records
* NS records
* A records
* CNAMEs
* MX records
* PTR records

Exam Tips

* You can buy domain names directly with AWS.
* It can take up to 3 days to register depending on the circumstances.

### Route53 Routing Policies Available on AWS

The following routing policies are available with Route53:

* Simple routing
* Weighted routing
* Latency-based routing
* Failover routing
* Geolocation routing
* Geoproximity routing (traffic flow only)
* Multivalue answer routing

### Simple routing policy

If you choose the simple routing policy you can only have one record with multiple IP addresses. If you specify multiple values in a record, Route53 returns all values to the user in a random order.

### Weighted routed policy

Allows you to split your traffic based on different weights assigned. For example, you can set 10% of your traffic to go to US-EAST-1 and 90% to go to EU-WEST-1.

#### Health checks

* You can set health checks on individual record sets.
* If a record set fails a health check it will be removed from Route53 until it passes the health check.
* You can set SNS notifications to alert you if a health check is failed.

### Latency-based routing

Allows you to route your traffic based on the lowest network latency for your end user (i.e, which region will give them the fastest response time).

To use latency-based routing, you create a latency resource record set for the Amazon EC2 (or ELB) resource in each region that hosts your website. When Amazon Route53 receives a query for your site, it selects the latency resource record set for the region that gives the user the lowest latency. Route53 then responds with the value associated with that resource record set.

### Failover routing policy

Failover routing policies are used when you want to create an active/passive setup. For example, you may want your primary site to be in EU-WEST-2 and your secondary DR Site in AP-SOUTHEAST-2.

Route53 will monitor the health of your primary site using a health check.

A health check monitors the health of your end points.

### Geolocation routing policy

Geolocation routing lets you choose where your traffic will be sent based on the geographic location of your users (i.e., the location from which DNS queries originate). For example, you might want all queries from Europe to be routed to a fleet of EC2 instances that are specifically configured for your European customers. These servers may have the local language of your European customers and all prices are displayed in Euros.

### Geoproximity routing (Traffic Flow only)

Geoproximity routing lets Amazon Route53 route traffic to your resources based on the geographic location of your users and your resources. You can also optionally choose to route more traffic or less to a given resource by specifying a value, known as a bias. A bias expands or shrinks the size of the geographic region from which traffic is routed to a resource.

To use geoproximity routing, you must use Route53 traffic flow.

### Multivalue answer routing

Multivalue answer routing lets you configure Amazon Route 53 to return multiple values, such as IP addresses for your web servers, in response to DNS queries. Route 53 responds to DNS queries with up to eight healthy records and gives different answers to different DNS resolvers. The choice of which to use is left to the requesting service effectively creating a form or randomisation.

## VPCs

What is a VPC?

Amazon Virtual Private Cloud (Amazon VPC) lets you provision a logically isolated section of the Amazon Web Services (AWS) Cloud where you can launch AWS resources in a virtual network that you define. You can have complete control over your virtual networking environment, including selection of your own IP address range, creation of subnets, and configuration of route tables and network gateways.

You can easily customize the network configuration for your Amazon Virtual Private Cloud. For example, you can create a public-facing subnet for your webservers that has access to the Internet, and place your backend systems such as databases or application servers in a private-facing subnet with no Internet access. You can leverage multiple layers of security, including security groups and network access control lists, to help control access to Amazon EC2 instances in each subnet.

Additionally, you can create a Hardware Virtual Private Network (VPN) connection between your corporate datacenter and your VPC and leverage the AWS cloud as an extension of your corporate datacenter.

[VPC Image]

What can we do with a VPC?

* Launch instances into a subnet of your choosing
* Assign custom IP address ranges in each subnet
* Configure route tables between subnets
* Create internet gateway and attach it to our VPC
* Much better security control over your AWS resources
* Instance security groups
* Subnet network access control lists (ACLs)

Default VPC vs Custom VPC

* Default VPC is user friendly, allowing you to immediately deploy instances.
* All subnets in default VPC have a route our to the internet.
* Each EC2 instance has both a public and private IP address.

VPC Peering

* Allows you to connect one VPC with another via a direct network route using private IP addresses.
* Instances behave as if they were on the same private network.
* You can peer VPCs with other AWS accounts as well as with other VPCs in the same account.
* Peering is in a star configuration: i.e. 1 central VPC peers with 4 others. NO TRANSITIVE PEERING!!!
* You can peer between regions.

Remember the following:

* Think of a VPC as a logical datacenter in AWS.
* Consists of IGWs (or Virtual Private Gateways), route tables, network access control lists, subnets, and Security Groups
* 1 subnet = 1 availability zone
* Security groups are stateful; network access control lists are stateless
* NO TRANSITIVE PEERING

Remember the following:

* When you create a VPC, you get a default route table, Network Access Control List (NACL), and a default Security Group.
* It won't create any subnets, nor will it create a default internet gateway.
* US-East-1A in your AWS account can be a completely different availability zone to US-East-1A in another AWS account. The AZs are randomized.
* Amazon always reserve 5 IP addresses within your subnets.
* You can only have 1 Internet Gateway per VPC.
* Security Groups can't span VPCs.

### NAT Instances & NAT Gateways

NAT Instances Exam Tips

* When creating a NAT instance, disable Source/Destination Check on the instance.
* NAT instances must be in a public subnet.
* There must be a route out of the private subnet to the NAT instance, in order for this to work.
* The amount of traffic that NAT instances can support depends on the instance size. If you are bottlenecking, increase the instance size.
* You can create high availability using Autoscaling Groups, multiple subnets in different AZs, and a script to automate failover.
* Behind a Security Group.

NAT Gateways

* Redundant inside the Availability Zone
* Preferred by the enterprise
* Starts at 5 Gbps and scales currently to 45 Gbps
* No need to patch
* Not associated with security groups
* Automatically assigned a public IP address
* Remember to update your route tables
* No need to disable Source/Destination Checks
* If you have resources in multiple Availability Zones and they share one NAT gateway, in the event that the NAT gateway's Availability Zone is down, resources in other Availability Zones lose internet access. To create an Availability Zone-independent architecture, create a NAT gateway in each Availability Zone and configure your routing to ensure that resources use the NAT gateway in the same Availability Zone.

Network Access Control Lists vs. Security Groups

* Your VPC automatically comes with a default network ACL, and by default it allows all outbound and inbound traffic.
* You can create custom network ACLs. By default, each custom network ACL denies all inbound and outbound traffic until you add rules.
* Each subnet in your VPC must be associated with a network ACL. If you don't explicitly associate a subnet with a network ACL, the subnet is automatically associated with the default network ACL.
* Block IP Addresses using network ACLs not Security Groups.
* You can associate a network ACL with multiple subnets; however, a subnet can be associated with only one network ACL at a time. When you associate a network ACL with a subnet, the previous association is removed.
* Network ACLs contain a numbered list of rules that is evaluated in order, starting with the lowest numbered rule.
* Network ACLs have separate inbound and outbound rules, and each rule can either allow or deny traffic.
* Network ACLs are stateless; responses to allowed inbound traffic are subject to the rules for outbound traffic (and vice versa).

Custom VPCs and ELBs

#### VPC Flow Logs

VPC Flow Logs is a feature that enables you to capture information about the IP traffic going to and from network interfaces in your VPC. Flow log data is stored using Amazon CloudWatch Logs. After you've created a flow log, you can view and retrieve its data in Amazon CloudWatch Logs.

Flow logs can be created at 3 levels:

* VPC
* Subnet
* Network Interface Level

Remember the following:

* YOu cannot enable flow logs for VPCs that are peered with your VPC unless the peer VPC is in your account.
* You cannot tag a flow log.
* After you've created a flow log, you cannot change its configuration; for example, you can't associate a different IAM role with the flow log.

Not all traffic is monitored:

* Traffic generated by instances when they contact the Amazon DNS server. If you use your own DNS server, then all traffic to that DNS server is logged.
* Traffic generated by a Windows instance for Amazon Windows license activation.
* Traffic to and from 169.254.169.254 for instance metadata.
* DHCP traffic.
* Traffic to the reserved IP address for the default VPC router.

#### Bastion Hosts

A bastion host is a special purpose computer on a network specifically designed and configured to withstand attacks. The computer generally hosts a single application, for example a proxy server, and all other services are removed or limited to reduce the threat to the computer. It is hardened in this manner primarily due to its location and purpose, which is either on the outside of a firewall or in a demilitarized zone (DMZ) and usually involves access from untrusted networks or computers. [Wikipedia]

* A NAT gateway or NAT instance is used to provide internet traffic to EC2 instances in a private subnet.
* A bastion (or jump box) is used to securely administer EC2 instances (using SSH or RDP).
* You cannot use a NAT gateway as a bastion host.

#### Direct Connect

AWS Direct Connect is a cloud service solution that makes it easy to establish a dedicated network connection from your premises to AWS. Using AWS Direct Connect, you can establish private connectivity between AWS and your datacenter, office, or colocation environment, which in many cases can reduce your network costs, increase bandwidth throughput, and provide a more consistent network experience than Internet-based connections.

* Direct Connect directly connects your data center to AWS
* Useful for highh throughput workloads (i.e., lots of network traffic)
* Or if you need a stable and reliable secure connection.

#### VPC Endpoints

A VPC Endpoint enables you to privately connect your VPC to supported AWS services and VPC endpoint services powered by PrivateLink without requiring an internet gateway, NAT device, VPN connection, or AWS Direct Connect connection. Instances in your VPC do not require public IP addresses to communicate with resources in the service. Traffic between your VPC and the other service does not leave the Amazon network.

Enpoints are virtual devices. They are horizontally scaled, redundant, and highly available VPC components that allow communication between instances in your VPC and services without imposing availability risks or bandwidth constraints on your network traffic.

There are two types of VPC endpoints:

* Interface Endpoints
* Gateway Endpoints

An interface endpoint is an elastic network interface with a private IP address that serves as an entry point for traffic destined to a supported device. The following services are supported:

* Amazon API Gateway
* AWS CloudFormation
* Amazon CloudWatch
* Amazon CloudWatch Events
* Amazon CloudWatch Logs
* AWS CodeBuild
* AWS Config
* Amazon EC2 API
* Elastic Load Balancing API
* AWS Key Management Service
* Amazon Kinesis Data Streams
* Amazon SageMaker and Amazon SageMaker Runtime
* Amazon SageMaker Notebook Instance
* AWS Secrets Manager
* AWS Security Token Service
* AWS Service Catalog
* Amazon SNS
* Amazon SQS
* AWS Systems Manager
* Endpoint services hosted by other AWS accounts
* Supported AWS Marketplace partner services

Currently Gateway Endpoints support:

* Amazon S3
* DynamoDB

#### Quiz

* The purpose of an "Egress-Only Internet Gateway" is to allow IPv6 based traffic within a VPC to access the Internet, whilst denying any Internet based resources the possibility of initiating a connection back into the VPC.
* When connecting a VPN between AWS and a third party site, the Customer Gateway is created within AWS, but it contains information about the third party site e.g. the external IP address and type of routing. The Virtual Private Gateway has the information regarding the AWS side of the VPN and connects a specified VPC to the VPN. "Direct Connect Gateway" and "Cross Connect" are both Direct Connect related terminology and have nothing to do with VPNs.

## HA ARCHITECTURE

### Elastic Load Balancers

Load Balancer types:

1. Application Load Balancer
2. Network Load Balancer
3. Classic Load Balancer

**Application Load Balancers** are best suited for load balancing of HTTP and HTTPS traffic. They operate at Layer 7 and are application-aware. They are intelligent, and you can create advanced request routing, sending specified requests to specific web servers.

**Network Load Balancers** are best suited for load balancing of TCP traffic where **extreme performance** is required. Operating at the connection level (Layer 4), Network Load Balancer are capable of handling millions of requests per second, while maintaining ultra-low latencies.

**Classic Load Balancers** are the legacy Elastic Load Balancers. You can load balance HTTP/HTTPS applications and use Layer 7-specific features, such as X-Forwarded and sticky sessions. You can also use strict Layer 4 load balancing for applications that rely purely on the TCP protocol.

Errors in Load Balancing

Classic Load Balancers - if your application stops responding, the ELB (Classic Load Balancer) responds with a **504 error** (gateway timeout). This means that the application is having issues. This could be either at the web server layer or at the database layer. Identify where the application is failing, and scale it up or out where possible.

* If you need the IPv4 address of your end user, look for the **X-Forwarded-For** header.
* Instances monitored by ELB are reported as InService or OutofService
* Health Checks check the instance health by talking to it
* Load balancers have their own DNS name, you are never given an IP address
* Read the ELB FAQ for Classic Load Balancers

### Advanced Load Balancer Theory

#### Sticky Sessions

Classic Load Balancer routes each request independently to the registered EC2 instance with the smallest load. Sticky sessions allow you to bind a user's session to a specific EC2 instance. This ensures that all requests from the user during the session are sent to the same instance.

You can enable Sticky Sessions for Application Load Balancers as well, but the traffic will be sent at the Target Group Level.

#### What is Cross Zone Load Balancing

#### Path Patterns

You can create a listener with rules to forward requests based on the URL path. This is known as path-based routing. If you are running microservices, you can route traffic to multiple back-end services using path-based routing. For example, you can route general requests to one target group and requests to render images to another target group.

#### Exam Tips

* Sticky Sessions enable your users to stick to the same EC2 instance. Can be useful if you are storing information locally to that instance.
* Cross Zone Load Balancing enables you to load balance across multiple availability zones.
* Path patterns allow you to direct traffic to different EC2 instances based on the URL contained in the request.

### Launch Configurations & Auto Scaling Groups

### HA Architecture

Scenario: You have a website that requires a minimum of 6 instances and it must be highly available. You must also be able to tolerate the failure of 1 Availability Zone. What is the ideal architecture for this environment while also being the most cost effective?

* 2 Availability Zones with 2 instances in each AZ
* **3 Availability Zones with 3 instances in each AZ**
* 1 Availability Zone with 6 instances in each AZ
* 3 Availability Zones with 2 instances in each AZ

Remember the following:

* Always design for failure
* Use multiple AZs and multiple regions wherever you can
* Know the difference between Multi-AZ and Read Replicas for RDS
* Know the difference between scaling out and scaling up
* Read the question carefully and always consider the cost element
* Know the different S3 storage classes

### Building a Fault Tolerant Wordpress Site

### CloudFormation

* Is a way of completely scripting your cloud environment
* Quick Start is a bunch of CloudFormation templates already built by AWS Solutions Architects allowing you to create complex environments very quickly.

### Elastic Beanstalk

With Elastic Beanstalk, you can quickly deploy and manage applications in the AWS Cloud without worrying about the infrastructure that runs those applications. You simply upload your application, and Elastic Beanstalk automatically handles the details of capacity provisioning, load balancing, scaling, and application health monitoring.

### Quiz

**Q1.** You need to use an object-based storage solution to store your critical, non-replaceable data in a cost-effective way. THis data will be frequently updated and will need some form of version control enabled on it. Which S3 storage solution should you use?

* S3 - RRS
* S3 - IA
* Glacier
* S3 - OneZone-IA
* **S3**

**Q2.** When you have deployed a RDS database into multiple availability zones, can you use the secondary database as an independent read node?

* Only in US-West-1
* Yes
* **No**
* It depends on how you set it up

**Q3.** In S3 the durability of my files is...

* 99 percent
* 100 percent
* 99.99 percent
* **99.999999999 percent**

**Q4.** You manage a high-performance site that collects scientific data using a bespoke protocol over TCP port 1414. The data comes in at high speed and is distributed to an autoscaling group of EC2 computer services spread over three AZs. Which type of AWS Load Balancer would best meet this requirement?

* Classic Load Balancers (CLB)
* Application Load Balancers (ALB)
* **Network Load Balancers (NLB)**
* Elastic Load Balancers (ELB)
* CloudFront combined with Lambda@Edge

**Q5.** Following an unplanned outage, you have been called into a planning meeting. You are asked what can be done to reduce the risk of a single bad deployment taking the whole site down. (The selected options do not necessarily need to work together) (Choose 4)

* **Use a Classic Load Balancer to spread the load over several availability zones**
* Use an Application Load Balancer to spread the load over several regions
* **Use Route53 with health checks to distribute load across multiple ELBs**
* Use automation to ensure that all updates are always deployed to all autoscaling groups at the same time
* **Use several target groups or auto scaling groups under each load balancer**
* **Use multiple autoscaling groups and boundaries for a staged or 'canary' deployment process**
* Use Route53 to direct traffic to the multi-region compute services on a round-robin basis

**Q6.** A product manager walks into your office and advises that the single node MySQL RDS instance that has been used for a pilot needs to be upgraded for production. She also advises that they may need to alter the size of the instance once they see how many people use the system during peak periods. The key concern is that there can not be any outages of more than a few seconds during the go-live period. Which of the following might you recomment. (Choose 2)

* **Convert the RDS instance to a multi-AZ implementation**
* Upgrade the RDS instance to a large size before go-live to avoid the 10-15 minute outage needed to change size later
* Implement read-replicas now to allow the instance size to be altered on the fly without any user impact
* **Consider replacing it with Aurora before go live**

**Q7.** In discussions about Cloud services the words 'Availability', 'Durability', 'Reliability' and 'Resiliency' are often used. Which term is used to refer to the likelihood that a resource ability to recover from damage or disruption?

* Reliability
* Availability
* Durability
* **Resiliency**

**Q8.** You have a web site with three distinct services each hosted by different web server autoscaling groups. Which AWS service should you use.

* Network Load Balancers (NLB)
* Elastic Load Balancers (ELB)
* **Application Load Balancers (ALB)**
* S3 Static web sites
* Classic Load Balancers (CLB)

**Q9.** You work for a major news network in Europe. They have just released a new mobile app that allows users to post their photos of newsworthy events in real-time. Your organization expects this app to grow very quickly, essentially doubling its user base each month. The app uses S3 to store the images, and you are expecting sudden and sizable increases in traffic to S3 when a major news event takes place (as users will be uploading large amounts of content.) You need to keep your storage costs to a minimum, and you are happy to temporally lose access to up to 0.1% of uploads per year. With these factors in mind, which storage media should you use to keep costs as low as possible?

* S3 Standard
* **S3 Standard-IA**
* S3 - Reduced Redundancy Storage (RRS)
* S3 - OneZone-Infrequent Access
* Glacier
* S3 - Provisioned IOPS

**Q10.** In discussions about Cloud services the words 'Availability', 'Durability', 'Reliability' and 'Resiliency' are often used. Which term is used to refer to the likelihood that you can access a resource or service when you need it?

* Durability
* Resiliency
* Reliability
* **Availability**

**Q11.** You work for a manufacturing company that operate a hybrid infrastructure with systems located both in a local data center and in AWS, connected via AWS Direct Connect. Currently, all on-premise servers are backed up to a local NAS, but your CTO wants you to decide on the best way to store copies of these backups in AWS. He has asked you to propose a solution which will provide access to the files within milliseconds should they be needed, but at the same time minimizes cost. As these files will be copies of backups stored on-premise, availability is not as critical as durability. Choose the best option from the following which meets the brief.

* Copy the files to an EC2 instance with a large EBS volume attached.
* **Copy the files from the NAS to an S3 bucket with the One Zone-IA class**
* Copy the files from the NAS to an S3 bucket configured as Standard class.
* Copy the files from the NAS to an S3 bucket with the Reduced Redundancy Storage class.

**Q12.** If you are told that an EC2 instance is being changed to have more RAM, Is this considered Scaling Up or Scaling Out

* **Scaling Up**
* Scaling Out

**Q13.** In discussions about Cloud services the words 'Availability', 'Durability', 'Reliability' and 'Resiliency' are often used. Which term is used to refer to the likelihood that a resource will work as designed?

* **Reliability**
* Resiliency
* Availability
* Durability

**Q14.** In discussions about Cloud services the words 'Availability', 'Durability', 'Reliability' and 'Resiliency' are often used. Which term is used to refer to the likelihood that a resource will continue to exist until you decide to remove it?

* Availability
* Resiliency
* **Durability**
* Reliability

**Q15.** Can I "force" a failover for any RDS instance that has Multi-AZ configured?

* **Yes.**
* No.
* Only for Oracle RDS instances.

**Q16.** Placement Groups can either be of the type 'Cluster', 'Spread', or 'Partition'. Choose options from below which are only specific to Spread Placement Groups.

* An instance can be launched in one placement group at a time and cannot span multiple placement groups.
* Spread placement groups require a name that is unique within your AWS account for the region
* A spread placement group is a logical grouping of instances within a single Availability Zone
* **A spread placement group is a group of instances that are each placed on distinct underlying hardware**

## APPLICATIONS

### SQS

Amazon SQS is a web service that gives you access to a message queue that can be used to store messages while waiting for a computer to process them.

Amazon SQS is a distributed queue system that enables web service applications to quickly and reliably queue messages that one component in the application generates to be consumed by another component. A queue is a temporary repository for messages that are awaiting processing.

Using Amazon SQS, you can decouple the components of an application so they run independently, easing message management between components. Any component of a distributed application can store messages in a fail-safe queue. Messages can contain up to 256 KB of text in any format. Any component can later retrieve the messages programmatically using the Amazon SQS API.

The queue acts as a buffer between the component producing and saving data, and the component receiving the data for processing. This means the queue resolves issues that arise if the producer is producing work faster than the consumer can process it, or if the producer or consumer are only intermittently connected to the network.

There are two types of Queue:

* Standard Queues (default)
* FIFO Queues

Amazon SQS offers standard as the default queue type. A standard queue lets you have a nearly-unlimited number of transactions per second. Standard queues guarantee that a message is delivered at least once. However, occasionally (because of the highly-distributed architecture that allows high throughput), more than one copy of a message might be delivered out of order. Standard queues provide best-effort ordering which ensures that messages are generally delivered in the same order as they are sent.

The FIFO queue complements the standard queue. The most important features of this queue type are FIFO (first-in-first-out) delivery and exactly-once processing: The order in which messages are sent and received is strictly preserved and a message is delivered once and remains available until a consumer processes and deletes it; duplicates are not introduced into the queue.

FIFO queues also support message groups that allow multiple ordered message groups within a single queue. FIFO queues are limited to 300 transactions per second (TPS), but have all the capabilities of standard queues.

#### SQS Exam Tips

* SQS is a way to de-couple your infrastructure
* SQS is pull based, not push based
* Message are 256 KB in size
* Message can be kept in the queue from 1 minute to 14 days; the default retention period is 4 days.
* Standard SQS and FIFO SQS
* Standard order is not guaranteed and messages can be delivered more than once
* FIFO order is strictly maintained and messages are delivered only once
* Visibility Time Out is the amount of time that the message is invisible in the SQS queue after a reader picks up that message. Provided the job is processed before the visibility time out expires, the message will then be deleted from the queue. If the job is not processed within that time, the message will become visible again and another reader will process it. This could result in the same message being delivered twice.
* Visibility timeout maximum is 12 hours.
* SQS guarantees that your messages will be processed at least once.
* Amazon SQS long polling is a way to retrieve messages from your Amazon SQS queues. While the regular short polling returns immediately (even if the message queue being polled is empty), long polling doesn't return a response until a message arrives in the message queue, or the long poll times out.

### Simple Work Flow Service (SWF)

Amazon Simple Workflow Service (Amazon SWF) is a web service that makes it easy to coordinate work across distributed application components. SWF enables applications for a range of use cases, including media processing, web application backends, business process workflows, and analytics pipelines, to be designed as a coordination of tasks.

Tasks represent invocations of various processing steps in an application which can be performed by executable code, web service calls, human actions, and scripts.

#### SWF vs SQS

* SQS has a retention period of up to 14 days; with SWF, workflow executions can last up to 1 year.
* Amazon SWF presents a task-oriented API, whereas Amazon SQS offers a message-oriented API.
* Amazon SWF ensures that a task is assigned only once and is never duplicated. With Amazon SQS, you need to handle duplicated messages and may also need to ensure that a message is processed only once.
* Amazon SWF keeps track of all the tasks and events in an application. With Amazon SQS, you need to implement your own application-level tracking, especially if your application uses multiple queues.

#### SWF Actors

* Workflow Starters - An application that can initiate (start) a workflow. Could be your e-commerce website following the placement of an order, or a mobile app searching for bus times.
* Deciders - Control the flow of activity tasks in a workflow execution. If something has finished (or failed) in a workflow, a Decider decides what to do next.
* Activity Workers - Carry out the activity tasks.

### Simple Notification Service (SNS)

Amazon Simple Notification Service (Amazon SNS) is a web service that makes it easy to set up, operate, and send notifications from the cloud.

It provides developers with a highly scalable, flexible, and cost-effective capability to publish messages from an application and immediately deliver them to subscribers or other applications.

#### Push notifications

Push notifications to Apple, Google, Fire OS, and Windows devices, as well as Android devices in China with Baidu Cloud Push.

#### SQS integration

Besides pushing cloud notifications directly to mobile devices, Amazon SNS can also deliver notifications by SMS text message or email to Amazon Simple Queue Service (SQS) queues, or to any HTTP endpoint.

#### What is a Topic?

SNS allows you to group multiple recipients using topics. A topic is an "access point" for allowing recipients to dynamically subscribe for identical copies of the same notification.

One topic can support deliveries to multiple endpoint types - for example, you can group together iOS, Android and SMS recipients. When you publish once to a topic, SNS delivers appropriately formatted copies of your message to each subscriber.

#### SNS Availability

To prevent messages from being lost, all messages published to Amazon SNS are stored redundantly across multiple availability zones.

#### Exam Tips

SNS Benefits

* Instantaneous, push-based delivery (no polling)
* Simple APIs and easy integration with applications
* Flexible message delivery over multiple transport protocols
* Inexpensive, pay-as-you-go model with no up-front costs
* Web-based AWS Management Console offers the simplicity of a point-and-click interface

SNS vs SQS

* Both are messaging services in AWS
* SNS - Push
* SQS - Polls (Pulls)

### Elastic Transcoder

What is Elastic Transcoder?

* Media transcoder in the cloud
* Convert media files from their original source format in to different formats that will play on smartphones, tablets, PCs, etc.
* Provides transcoding presets for popular output formats, which means that you don't need to guess about which settings work best on particular devices.
* Pay based on the minutes that you transcode and the resolution at which you transcode.

### API Gateway

Amazon API Gateway is a fully managed service that makes it easy for developers to publish, maintain, monitor, and secure APIs at any scale.

With a few clicks in the AWS Management Console, you can create an API that acts as a "front door" for applications to access data, business logic, or functionality from your back-end services, such as applications running on Amazon Elastic Compute Cloud (Amazon EC2), code running on AWS Lambda, or any web application.

#### What can API Gateway do?

* Expose HTTPS endpoints to define a RESTful API
* Serverless-ly connect to services like Lambda & DynamoDB
* Send each API endpoint to a different target
* Run efficiently with low cost
* Scale effortlessly
* Track and control usage by API key
* Throttle requests to prevent attacks
* Connect to CloudWatch to log all requests for monitoring
* Maintain multiple versions of yout API

#### API Gateway configuration

* Define an API (container)
* Define Resources and nested Resources (URL paths)

For each resource,

* select supported HTTP methods (verbs)
* set security
* choose target (such as EC2, Lambda, DynamoDB, etc.)
* set request and response transformations

#### API Gateway Deployments

Deploy API to a stage:

* Uses API Gateway domain, by default
* Can use custom domain
* Now supports AWS Certificate Manager: free SSL/TLS certs

#### API Gateway Caching

You can enable API caching in Amazon API Gateway to cache your endpoint's response. With caching, you can reduce the number of calls made to your endpoint and also improve the latency of the requests to your API. When you enable caching for a stage, API Gateway caches responses from your endpoint for a specific time-to-live (TTL) period, in seconds. API Gateway then responds to the request by looking up the endpoint response from the cache instead of making a request to your endpoint.

#### Same Origin Policy

In computing, the same-origin policy is an important concept in the web application security model. Under the policy, a web browser permits scripts contained in a first web page to access data in a second web page, but only if both web pages have the same origin.

This is done to prevent Cross-Site Scripting (XSS) attacks.

* Enforced by web browsers.
* Ignored by tools like PostMan and curl.

#### CORS Explained

CORS is one way the server at the other end (not the client code in the browser) can relax the same-origin policy.

Cross-Origin Resource Sharing (CORS) is a mechanism that allows restricted resources (e.g. fonts) on a web page to be requested from another domain outside the domain from which the first resource was served.

#### CORS in Action

* Browser makes an HTTP OPTIONS call for a URL (OPTIONS is an HTTP method like GET, PUT, and POST)
* Server returns a response that says: "These other domains are approved to GET this URL."
* Error - "Origin policy cannot be read at the remote resource?" You need to enable CORS on API Gateway.

#### API Gateway Exam Tips

* Remember what API Gateway is at a high level
* API Gateway has caching capabilities to increase performance
* API Gateway is low cost and scales automatically
* You can throttle API Gateway to prevent attacks
* You can log results to CloudWatch
* If you are using Javascript/AJAX that uses multiple domains with API Gateway, ensure that you have enabled CORS on API Gateway
* CORS is enforced by the client

### Kinesis

What is Streaming Data?

Streaming Data is data that is generated continuously by thousands of data sources, which typically send in the data records simultaneously, and in small sizes (order of Kilobytes.)

* Purchases from online stores
* Stock prices
* Game data (as the gamer plays)
* Social network data
* Geospatial data (think uber.com)
* IoT sensor data

#### What is Kinesis?

Amazon Kinesis is a platform on AWS to send your streaming data to. Kinesis makes it easy to load and analyze streaming data, and also providing the ability for you to build your own custom applications for your business needs.

3 different types of Kinesis:

* Kinesis Streams
* Kinesis Firehose
* Kinesis Analytics

#### Kinesis Streams

Kinesis Streams consist of Shards:

* 5 transactions per second for reads, up to a maximum total data read rate of 2 MB per second and up to 1,000 records per second for writes, up to a maximum total data write rate of 1 MB per second (including partition keys.)
* The data capacity of your stream is a function of the number of shards that you specify for the stream. The total capacity of the stream is the sum of the capacities of its shards.

#### Kinesis Firehose

#### Kinesis Analytics

#### Kinesis Exam Tips

* Know the difference between Kinesis Streams and Kinesis Firehose. You will be given scenario questions and you must choose the most relevant service.
* Understand what Kinesis Analytics is.

### Web Identity Federation & Cognito

#### What is Web Identity Federation?

Web Identity Federation lets you give your users access to AWS resources after they have successfully authenticated with a web-based identity provider like Amazon, Facebook, or Google. Following successful authentication, the user receives an authentication code from the Web ID provider, which thay can trade for temporary AWS security credentials.

#### Amazon Cognito

Amazon Cognito provides Web Identity Federation with the following features:

* Sign-up and sign-in to your apps
* Access for guest users
* Acts as an Identity Broker between your application and Web ID providers, so you don't need to write any additional code.
* Synchronizes user data for multiple devices
* Recommended for all mobile application AWS services (???)

#### Amazon Cognito Use Cases

The recommended approach for Web Identity Federation using social media acconts like Facebook.

Cognito brokers between the app and Facebook or Google to provide temporary creadentials which map to an IAM role allowing access to the required resources.

No need for the application to embed or store AWS credentials locally on the device and it gives users a seamless experience across all mobile devices.

#### Cognito User Pools

User Pools are user directories used to manage sign-up and sign-in functionality for mobile and web applications. Users can sign-in directly to the User Pool, or using Facebook, Amazon, or Google. Cognito acts as an Identity Broker between the identity provider and AWS. Successful authentication generates a JSON Web token (JWTs).

#### Cognito Identity Pools

Identity Pools enable provide temporary AWS credentials to access AWS services like S3 or DynamoDB.

#### Cognito Synchronisation

Cognito tracks the association between user identity and the various different devices they sign-in from. In order to provide a seamless user experience for your application, Cognito uses Push Synchronization to push updates and synchronize user data across multiple devices. Cognito uses SNS to send a notification to all the devices associated with a given user identity whenever data stored in the cloud changes.

#### Cognito Exam Tips

* Federation allows users to authenticate with a Web Identity Provider (Google, Facebook, Amazon)
* The user authenticates first with the Web ID Provider and receives an authentication token, which is exchanged for temporary AWS credentials allowing them to assume an IAM role.
* Cognito is an Identity Broker which handles interaction between your applications and the Web ID provider (you don't need to write your own code to do this.)
* User pool is user based. It handles things like user registration, authentication, and account recovery.
* Identity pools authorise access to your AWS resources.

### Applications Quiz

**Q1.** What does Amazon SES stand for?

* Software Enabled Server
* Simple Elastic Server
* Software Email Solution
* **Simple Email Service**

**Q2.** By default, EC2 instances pull SQS messages from a standard SQS queue on a FIFO (First In First out) basis.

* **False**
* True

**Q3.** What is the difference between SNS and SQS?

* SNS is a push notification service, whereas SQS is message system that requires worker nodes to poll a queue.
* SQS sends messages to people on topics, whereas SNS manages tasks.
* SNS pulls (polls), whereas SQS is push-based message service.
* SQS and SNS are basically the same service.

**Q4.** Amazon SWF restricts me to the use of specific programming languages.

* True
* **False**

**Q5.** What happens when you create a topic on Amazon SNS?

* **An Amazon Resource Name is created.**
* The topic will terminate your EC2 instances that aren't identified by tags.
* You cannot create a topic on SNS.
* Nothing, as topics are specific to Amazon SQS.

**Q6.** Amazon SWF is designed to help users ...

* **Coordinate synchronous and asynchronous tasks**
* Secure their VPCs
* Manage user identification and authorization
* Store file-based objects

**Q7.** In SWF, what does a "domain" refer to?

* A specialized security group configuration
* The DNS record for the Amazon SWF service
* A special type of worker instance
* **A collection of related workflows**

**Q8.** What dows Amazon SWF stand for?

* **Simple Work Flow**
* Simple Wireless Forms
* Simple Web Form
* Simple Web Flow

**Q9.** Amazon's SQS service guarantees a message will be delivered at least once.

* **True**
* False

**Q10.** What application service allows you to decouple your infrastructure using messaged based queues?

* SNS
* SES
* SWF
* **SQS**

**Q11.** Amazon SWF ensures that a task is assigned only once and is never duplicated.

* False
* **True**

## SERVERLESS

### Lambda

Lambda is the ultimate anstraction layer:

* Data centers
* Hardware
* Assembly code / Protocols
* High level languages
* Operating systems
* Application layer / AWS APIs
* AWS Lambda

AWS Lambda is a compute service where you can upload your code and create a Lambda function. AWS Lambda takes care of provisioning and managing the servers that you use to run the code. You don't have to worry about operating systems, patching, scaling, etc.

You can use Lambda in the following ways:

* As an event-driven compute service where AWS Lambda runs your code in response to events. These events could be changes to data in an Amazon S3 bucket or an Amazon DynamoDB table.
* As a compute service to run your code in response to HTTP requests using Amazon API Gateway or API calls made using AWS SDKs.

What Languages does Lambda Support?

* Node.js
* Java
* Python
* C#
* Go
* PowerShell

How is Lambda priced?

1. Number of requests: First 1 million requests are free. $0.20 per 1 million requests thereafter.
2. Duration - Duration is calculated from the time your code begins executing until it returns or otherwise terminates, rounded up to the nearest 100ms. The price depends on the amount of memory you allocate to your function. You are charged $0.00001667 for every GB-second used.

Why is Lambda cool?

* NO SERVERS!
* Continuous scaling
* Super super super cheap!

Lambda Exam Tips

* Lambda scales out (not up) automatically
* Lambda functions are independent, 1 event = 1 function
* Lambda is serverless
* Know what services are serverless!
* Lambda functions can trigger other lambda functions, 1 event can = x functions if functions trigger other functions
* Architectures can get extremely complicated, AWS X-ray allows you to debug what is happening
* Lambda can do things globally, you can use it to back up S3 buckets to other S3 buckets, etc.
* Know your triggers

### Quiz

**Q1.** In which direction(s) does Lambda scale automatically?

* **Out**
* Up
* None - Lambda does not scale automatically
* Up and Out

**Q2.** What AWS service can be used to help resolve an issue with a Lambda function?

* DynamoDB
* **AWS X-Ray**
* API Gateway
* CloudTrail

**Q3.** What does the common term 'Serverless' mean according to AWS (Choose 2)

* The use of Quantum computing to eliminate the need for physical servers.
* A marketing term for HaaS (Hosting as a Service).
* **A native Cloud Architecture that allows customers to shift more operational responsibility to AWS.
* **The ability to run applications and services without thinking about servers or capacity provisioning.
* A pricing model based on high level commodity measures such as on compute duration and storage capacity.

**Q4.** You have created a serverless application to add metadata to images that are uploaded to a specific S3 bucket. To do this, your lambda function is configured to trigger whenever a new image is created in the bucket. What will happen when multiple users upload multiple different images at the same time?

* Multiple Lambda functions will trigger, one after the other, until all images are processed
* A single Lambda functions will be triggered, which will process all images at the same time
* **Multiple instances of the Lambda function will be triggered, one for each image**
* A single Lambda functions will be triggered, that will process all images that have finished uploading one at a time

**Q5.** On Friday morning your marketing manager calls an urgent meeting to celebrate that they have secured a deal to run a coordinated national promotion on TV, radio, and social media over the next 10 days. They anticipate a 500x increase on site visits and trial registrations. After the meeting you throw some ideas around with your team about how to ensure that your current 1 server web site will survive. Which of these best embody the AWS design strategy for this situation. (Choose 2)

* Work with your web design team to create some web pages in PHP to run on a 32xlarge EC2 instance to emulate your 5 most popular information web pages and sign up web pages.
* Recreate your 5 most popular new customer web pages and sign up web pages on Lightsail and take advantage of AWS auto scaling to pick up the load.
* **Create a duplicate sign up page that stores registration details in DynamoDB for asynchronous processing using SQS & Lambda.**
* Create a stand by sign up server to use in case the primary fails due to load.
* Upgrade your existing server from a 1xlarge to a 32xlarge for the duration of the campaign.
* **Work with your web design team to create some web pages with embedded JavaScript to emulate your 5 most popular information web pages and sign up web pages.**

**Q6.** Which of the following services can invoke Lambda function directly? (Choose 3)

* Elastic Load Balancer
* EC2
* **DynamoDB**
* IAM
* **S3**
* **API Gateway**

**Q7.** You have created a simple serverless website using S3, Lambda, API Gateway and DynamoDB. Your website will process the contact details of your customers, predict an expected delivery date of their order and store their order in DynamoDB. You test the website before deploying it into production and you notice that although the page executes, and the lambda function is triggered, it is unable to write to DynamoDB. What could be the cause of this issue?

* The availability zone that DynamoDB is hosted in is down.
* **Your lambda function does not have sufficient Identity Access Management (IAM) permissions to write to DynamoDB.**
* The availability zone that Lambda is hosted in is down.
* You have written your function in Python which is not supported as a runtime environment for Lambda.

**Q8.** As a DevOps engineer you are told to prepare complete solution to run a piece of code that required multi-threaded processing, The code has been running on an old custom built server based around a 4 core Intel Xeon processor. Which of these best describe the AWS compute services that could be used?

* Only a EC2 'Bare Steel' server.
* None of the above.
* **EC2, ECS, & Lambda.**
* ECS, and EC2.

**Q9.** Lambda pricing is based on which of these measurements? (Choose 2)

* Whether you choose an AMD or Intel processor.
* **Duration of execution billed in fractions of seconds.**
* The amount of CPU assigned.
* **The amount of memory assigned.**

