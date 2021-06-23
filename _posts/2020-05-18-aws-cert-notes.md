---
layout: post
sitemap: true
noindex: false
title: Notes while studing for AWS Certification
categories: aws developer-certification 
---

Some notes while studing for AWS Developer Certification...

# Table of Contents

[Table of Contents](#table-of-contents)
 - [Global Infrastructure](#global-infrastructure)
 - [General](#general)
 - [ElastiCache](#elasticache)
 - [Beanstalk](#aws-elastic-beanstalk)
 - [Lambda](#lambda)
 - [Kinesis](#kinesis)
 - [CloudWatch](#cloudwatch)
 - [Apigateway](#apigateway)
 - [S3](#s3)
   - [S3 Replication](#s3-replication)
   - [S3 Pre-Signed URLs](#s3-pre-signed-urls)
   - [S3 Storage Classes](#s3-storage-classes)
   - [S3 – Moving between storage classes](#s3-–-moving-between-storage-classes)
   - [S3 Object Lock & Glacier Vault Lock](#s3-object-lock-&-glacier-vault-lock)
 - [AWS Athena](#aws-athena)
 - [ECS - Elastic Container Service ](#ecs---elastic-container-service-)
   - [ECS Clusters Overview](#ecs-clusters-overview)
   - [Amazon ECS Cluster Capacity Providers](#amazon-ecs-cluster-capacity-providers)
   - [ECS Task Definitions](#ecs-task-definitions)
   - [ECS Service](#ecs-service)
   - [ECS Service with Load Balancer](#ecs-service-with-load-balancer)
   - [ECR](#ecr)
   - [Fargate](#fargate)
   - [ECS IAM Roles Deep Dive](#ecs-iam-roles-deep-dive)
   - [ECS Tasks Placement](#ecs-tasks-placement)
   - [ECS Cluster Capacity Provider](#ecs-cluster-capacity-provider)
   - [ECS Summary & Exam Tips](#ecs-summary-&-exam-tips)
 - [AWS Elastic Beanstalk](#aws-elastic-beanstalk)
   - [Elastic Beanstalk Overview](#elastic-beanstalk-overview)
   - [Elastic Beanstalk Deployment Modes](#elastic-beanstalk-deployment-modes)
   - [Elastic Beanstalk CLI](#elastic-beanstalk-cli)
   - [Beanstalk Other Stuff](#beanstalk-other-stuff)
   - [Beanstalk with HTTPS](#beanstalk-with-https)
   - [Beanstalk redirect HTTP to HTTPS](#beanstalk-redirect-http-to-https)
 - [Dynamodb](#dynamodb)
 - [AWS CICD - CodeCommit, CodePipeline, CodeBuild, CodeDeploy](#aws-cicd---codecommit,-codepipeline,-codebuild,-codedeploy)


# TO READ

 - [Best Practices for Designing and Architecting with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)
 - [Best Practices Design Patterns: Optimizing Amazon S3 Performance](https://d1.awsstatic.com/whitepapers/AmazonS3BestPractices.pdf?trk=wp_card)
 - [Best Practices for Working with AWS Lambda Functions](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
 - [Choosing the Right DynamoDB Partition Key](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/)
 - [How to perform ordered data replication between applications by using Amazon DynamoDB Streams](https://aws.amazon.com/blogs/database/how-to-perform-ordered-data-replication-between-applications-by-using-amazon-dynamodb-streams/)
 - [https://aws.amazon.com/about-aws/global-infrastructure](https://aws.amazon.com/about-aws/global-infrastructure)
 - [Comparing Redis and Memcached](https://aws.amazon.com/elasticache/redis-vs-memcached/)
 - [Caching Strategies](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html)
 - [Amazon Kinesis Data Streams Terminology and Concepts](https://docs.aws.amazon.com/streams/latest/dev/key-concepts.html)
 - [Amazon CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html)
 - [Setting up stage variables for a REST API deployment](https://docs.aws.amazon.com/apigateway/latest/developerguide/stage-variables.html)
 - [How Delete Operations Affect Replication - What Does Amazon S3 Replicate?](https://docs.aws.amazon.com/AmazonS3/latest/dev/replication-what-is-isnot-replicated.html)
 - [https://aws.amazon.com/s3/storage-classes](https://aws.amazon.com/s3/storage-classes)
 - S3 Select & Glacier Select   - [https://aws.amazon.com/blogs/aws/s3-glacier-select](https://aws.amazon.com/blogs/aws/s3-glacier-select/)
 - Serverless service to perform analytics directly against S3 files (unstructured, semi-structured, and structured data stored S3). [https://docs.aws.amazon.com/athena/index.html](https://docs.aws.amazon.com/athena/index.html)
 - [Amazon QuickSight](https://docs.aws.amazon.com/quicksight/latest/user/welcome.html) for easy data visualization. 
 - [AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html), which offers a persistent metadata store for your data in Amazon S3. 
 - [https://docs.aws.amazon.com/athena/latest/ug/querying-AWS-service-logs.html](https://docs.aws.amazon.com/athena/latest/ug/querying-AWS-service-logs.html)
 - Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service. Docs [https://aws.amazon.com/ecs/resources/](https://aws.amazon.com/ecs/resources/). There are three choices:
 - [AWS CLI ECS](https://docs.aws.amazon.com/cli/latest/reference/ecs/index.html)
 - [ECS CLI](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_reference.html)
 - [ECS Cluster Capacity Providers](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-capacity-providers.html)
 - [ECS Tasks definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html) are metadata in JSON form to tell ECS how to run a Docker Container. It contains crucial information around:
 - [ECS Services](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html) help define how many tasks should run and how they should be run. If any of your tasks should fail or stop for any reason, the ECS service scheduler launches another instance of your task definition to replace it in order to maintain the desired number of tasks in the service.
 - [AWS Elastic Beanstalk: Configuration options](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/command-options.html)
 - [AWS Elastic Beanstalk: Setting configuration options before environment creation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environment-configuration-methods-before.html)
 - [AWS Elastic Beanstalk - Deployment policies and settings](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rolling-version-deploy.html)
 - [AWS Elastic Beanstalk - Deployment policies](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html)
 - [AWS Elastic Beanstalk EB CLI](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-cmd-commands.html)
 - [Amazon DynamoDB Read/Write Capacity Mode](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html)



# Global Infrastructure

[https://aws.amazon.com/about-aws/global-infrastructure](https://aws.amazon.com/about-aws/global-infrastructure)

A region is a cluster of data centers: us-east-1, eu-west-3
Each region has many availability zones: ap-southeast-2a, ap-southeast-2b, ap-southeast-2c

[GoTo: Table of Contents](#table-of-contents)

# General

**Amazon Machine Image (AMI)**

You can select an AMI to use based on the following characteristics:
 - Region (see Regions, Availability Zones, and Local Zones)
 - Operating system: Ubuntu, Fedora, RedHat, Windows, Etc...
 - Architecture (32-bit or 64-bit)
 - Launch permissions (public, explicit, implicit)
 - Storage for the root device (backed by Amazon EBS or backed by instance store)
 - `describe-images` (AWS CLI)

[GoTo: Table of Contents](#table-of-contents)

# ElastiCache

[Comparing Redis and Memcached](https://aws.amazon.com/elasticache/redis-vs-memcached/)

[Caching Strategies](https://docs.aws.amazon.com/AmazonElastiCache/latest/mem-ug/Strategies.html)

[GoTo: Table of Contents](#table-of-contents)


# Lambda

[Best Practices for Working with AWS Lambda Functions](https://docs.aws.amazon.com/lambda/latest/dg/best-practices.html)
 - AWS Lambda function aliases

[GoTo: Table of Contents](#table-of-contents)

# Kinesis

[Amazon Kinesis Data Streams Terminology and Concepts](https://docs.aws.amazon.com/streams/latest/dev/key-concepts.html)

[GoTo: Table of Contents](#table-of-contents)

# CloudWatch
[Amazon CloudWatch Concepts](https://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/cloudwatch_concepts.html)

[GoTo: Table of Contents](#table-of-contents)

# Apigateway
[Setting up stage variables for a REST API deployment](https://docs.aws.amazon.com/apigateway/latest/developerguide/stage-variables.html)

[GoTo: Table of Contents](#table-of-contents)

# S3

[Best Practices Design Patterns: Optimizing Amazon S3 Performance](https://d1.awsstatic.com/whitepapers/AmazonS3BestPractices.pdf?trk=wp_card)

> Your applications can easily achieve thousands of transactions per second in request performance when uploading and retrieving storage from Amazon S3. Amazon S3 automatically scales to high request rates. For example, your application can achieve at least 3,500 PUT/COPY/POST/DELETE or 5,500 GET/HEAD requests per second per prefix in a bucket. There are no limits to the number of prefixes in a bucket. You can increase your read or write performance by parallelizing reads. For example, if you create 10 prefixes in an Amazon S3 bucket to parallelize reads, you could scale your read performance to 55,000 read requests per second.


[GoTo: Table of Contents](#table-of-contents)

### S3 Replication

   - Cross Region Replication (CRR)
   - Same Region Replication (SRR)
   - Must enable versioning in source and destination
   - Buckets can be in different accounts
   - Must give proper IAM permissions to S3
   - Copying is asynchronous
   - After activating, only new objects are replicated (not retroactive)
   - There is no “chaining” of replication
      - If bucket 1 has replication into bucket 2, which has replication into bucket 3, objects created in bucket 1 are not replicated to bucket 3
   - For DELETE operations:
      - If you delete without a version ID, it adds a delete marker, not replicated
      - If you delete with a version ID, it deletes in the source, not replicated
      - 

[How Delete Operations Affect Replication - What Does Amazon S3 Replicate?](https://docs.aws.amazon.com/AmazonS3/latest/dev/replication-what-is-isnot-replicated.html)

If you delete an object from the source bucket, the following occurs:

   - If you make a DELETE request without specifying an object version ID, Amazon S3 adds a delete marker. Amazon S3 deals with the delete marker as follows:
     - If you are using the latest version of the replication configuration (that is, you specify the Filter element in a replication configuration rule), Amazon S3 does not replicate the delete marker.
     - If you don't specify the Filter element, Amazon S3 assumes that the replication configuration is an earlier version V1. In the earlier version, Amazon S3 handled replication of delete markers differently. For more information, see Backward Compatibility.

   - If you specify an object version ID to delete in a DELETE request, Amazon S3 deletes that object version in the source bucket. But it doesn't replicate the deletion in the destination bucket. In other words, it doesn't delete the same object version from the destination bucket. This protects data from malicious deletions.

[GoTo: Table of Contents](#table-of-contents)

### S3 Pre-Signed URLs

   - Can generate pre-signed URLs using SDK or CLI
     - For downloads (easy, can use the CLI)
     - For uploads (harder, must use the SDK)
   - Valid for a default of 3600 seconds, can change timeout with   --expires-in [TIME_BY_SECONDS] argument
   - Users given a pre-signed URL inherit the permissions of the person who generated the URL for GET / PUT
   - Examples :
     - Allow only logged-in users to download a premium video on your S3 bucket
     - Allow an ever changing list of users to download files by generating URLs dynamically
     - Allow temporarily a user to upload a file to a precise location in our bucket

```bash
# setup: set the proper signature version in order not to get issues when generating URLs for encrypted files
aws configure set default.s3.signature_version s3v4

# always use   --region parameter!
aws s3 presign s3://mybucket/myobject   --region xxx

# adding a custom expiration time
aws s3 presign s3://mybucket/myobject   --region xxx   --expires-in 300 
```

[GoTo: Table of Contents](#table-of-contents)

### S3 Storage Classes

[https://aws.amazon.com/s3/storage-classes](https://aws.amazon.com/s3/storage-classes)

   - Amazon S3 Standard   - General Purpose
     - High durability, High availability, multi AZ
   - Amazon S3 Standard-Infrequent Access (IA)
     - the same as general purpose, um um .9% less, cheaper
     - Use Cases: As a data store for disaster recovery, backups
   - Amazon S3 One Zone-Infrequent Access
     - Same as IA but data is stored in a single AZ
     - Use Cases: Storing secondary backup copies of on-premise data, or storing data you can recreate
   - Amazon S3 Intelligent Tiering
     - Automatically moves objects between two access tiers based on changing access patterns
   - Amazon Glacier
     - Alternative to on-premise magnetic tape storage
     - Each item in Glacier is called "Archive" (up to 40TB)
     - Archives are stored in "Vaults"
      - retrieval options:
        - Expedited (1 to 5 minutes)
        - Standard (3 to 5 hours)
        - Bulk (5 to 12 hours)
        - Minimum storage duration of 90 days
   - Amazon Glacier Deep Archive
     - the same as Amazon Glacier
     - retrieval options:
        - Standard (12 hours)
        - Bulk (48 hours)
        - Minimum storage duration of 180 days
   - (deprecated) Amazon S3 Reduced Redundancy Storage 

[GoTo: Table of Contents](#table-of-contents)


#### S3 – Moving between storage classes

 You can transition objects between storage classes
   - For infrequently accessed object, move them to STANDARD_IA
   - For archive objects you don’t need in real-time, GLACIER or DEEP_ARCHIVE
   - Moving objects can be automated using a **lifecycle rules**

Transition actions: It defines when objects are transitioned to another storage class.

Config:
   - Rules can be created for a certain prefix (ex   - s3://mybucket/mp3/*)
   - Rules can be created for certain objects tags (ex   - Department: Finance)
Transition:
   - Move objects to Standard IA class 60 days after creation
   - Move to Glacier for archiving after 6 months
Expiration actions: configure objects to expire (delete) after some time
   - Access log files can be set to delete after a 365 days
   - Can be used to delete old versions of files (if versioning is enabled)
   - Can be used to delete incomplete multi-part uploads

S3 – Baseline Performance
   - application can achieve at least 3,500 PUT/COPY/POST/DELETE and 5,500 GET/HEAD requests per second **per prefix** in a bucket.
   - If you use SSE-KMS, you may be impacted by the KMS limits
     - KMS quota per second (5500, 10000, 30000 req/s based on region). Today, you cannot request a quota increase for KMS

   - Multi-Part upload: **recommended** for files > 100MB, **must use** for files > 5GB
    - S3 Transfer Acceleration (upload only): uses AWS edge location which will forward the data to the S3 bucket in the target region   - Compatible with multi-part upload

Other Stuff:
   - S3 Select & Glacier Select   - [https://aws.amazon.com/blogs/aws/s3-glacier-select](https://aws.amazon.com/blogs/aws/s3-glacier-select/)
   - S3 Event Notifications
     - S3:ObjectCreated, S3:ObjectRemoved, S3:ObjectRestore, S3:Replication…
     - Can Trigger SNS, SQS, Lambda. Object name filtering possible (*.jpg)
     - Use case: generate thumbnails of images uploaded to S3
   Can create as many “S3 events” as desired
     - Notes:
       - S3 event notifications typically deliver events in seconds but can sometimes take a minute or longer
       - If two writes are made to a single non-versioned object at the same time, it is possible that only a single event notification will be sent
       - If you want to ensure that an event notification is sent for every successful write, you can enable versioning on your bucket.

[GoTo: Table of Contents](#table-of-contents)

### S3 Object Lock & Glacier Vault Lock

S3 Object Lock & Glacier Vault Lock:

 - Adopt a WORM (Write Once Read Many) model
 - Block an object version deletion for a specified amount of time
 - Helpful for compliance and data retention

[GoTo: Table of Contents](#table-of-contents)

## AWS Athena

Serverless service to perform analytics directly against S3 files (unstructured, semi-structured, and structured data stored S3). [https://docs.aws.amazon.com/athena/index.html](https://docs.aws.amazon.com/athena/index.html)

   - Uses SQL language to query the files
   - Has a JDBC / ODBC driver
   - Charged per query and amount of data scanned
   - Supports CSV, JSON, ORC, Avro, and Parquet (built on Presto)
   - Use cases: Business intelligence / analytics / reporting, analyze & query VPC Flow Logs, ELB Logs, CloudTrail trails, etc.
   - Exam Tip: Analyze data directly on S3 => use Athena

- Athena integrates with: 
   - [Amazon QuickSight](https://docs.aws.amazon.com/quicksight/latest/user/welcome.html) for easy data visualization. 
   - [AWS Glue Data Catalog](https://docs.aws.amazon.com/glue/latest/dg/what-is-glue.html), which offers a persistent metadata store for your data in Amazon S3. 

It's also possible to use Amazon Athena to query popular AWS Service Logs, such as AWS CloudTrail logs, Amazon CloudFront logs, Classic Load Balancer logs, Application Load Balancer logs, Amazon VPC flow logs, and Network Load Balancer logs
[https://docs.aws.amazon.com/athena/latest/ug/querying-AWS-service-logs.html](https://docs.aws.amazon.com/athena/latest/ug/querying-AWS-service-logs.html)


```sql
create database s3_access_logs_db;

CREATE EXTERNAL TABLE IF NOT EXISTS s3_access_logs_db.mybucket_logs(
  BucketOwner STRING,
  Bucket STRING,
  RequestDateTime STRING,
  RemoteIP STRING,
  Requester STRING,
  RequestID STRING,
  Operation STRING,
  Key STRING,
  RequestURI_operation STRING,
  RequestURI_key STRING,
  RequestURI_httpProtoversion STRING,
  HTTPstatus STRING,
  ErrorCode STRING,
  BytesSent BIGINT,
  ObjectSize BIGINT,
  TotalTime STRING,
  TurnAroundTime STRING,
  Referrer STRING,
  UserAgent STRING,
  VersionId STRING)
ROW FORMAT SERDE 'org.apache.hadoop.hive.serde2.RegexSerDe'
WITH SERDEPROPERTIES (
'serialization.format' = '1',
  'input.regex' = '([^ ]*) ([^ ]*) \\[(.*?)\\] ([^ ]*) ([^ ]*) ([^ ]*) ([^ ]*) ([^ ]*) \\\"([^ ]*) ([^ ]*) (- |[^ ]*)\\\" (-|[0-9]*) ([^ ]*) ([^ ]*) ([^ ]*) ([^ ]*) ([^ ]*) ([^ ]*) (\"[^\"]*\") ([^ ]*)$'
)LOCATION 's3://target-bucket-name/prefix/';

SELECT * FROM "s3_access_logs_db"."mybucket_logs" limit 10;

SELECT * FROM "s3_access_logs_db"."mybucket_logs"
where httpstatus='403';

SELECT requesturi_operation, httpstatus, count(*) FROM "s3_access_logs_db"."mybucket_logs" 
GROUP BY requesturi_operation, httpstatus;

```

[GoTo: Table of Contents](#table-of-contents)

# ECS - Elastic Container Service 
----

Amazon Elastic Container Service (Amazon ECS) is a fully managed container orchestration service. Docs [https://aws.amazon.com/ecs/resources/](https://aws.amazon.com/ecs/resources/). There are three choices:
 - ECS: Amazon’s own platform
 - Fargate: Amazon’s own Serverless platform
 - EKS: Amazon’s managed Kubernetes (open source)

Utils:

 - [AWS CLI ECS](https://docs.aws.amazon.com/cli/latest/reference/ecs/index.html)
 - [ECS CLI](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_CLI_reference.html)

[GoTo: Table of Contents](#table-of-contents)


## ECS Clusters Overview

 - ECS Clusters are logical grouping of EC2 instances
 - EC2 instances run the ECS agent (Docker container)
 - The ECS agents registers the instance to the ECS cluster
 - The EC2 instances run a special AMI, made specifically for ECS

[GoTo: Table of Contents](#table-of-contents)

### Amazon ECS Cluster Capacity Providers

[] READ AND TAKE NOTES ON [Cluster Capacity Providers](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/cluster-capacity-providers.html)

Login in the cluster:

> cat /etc/ecs/ecs.config

```bash
ECS_CLUSTER=MyCluster     #Assign EC2 instance to an ECS cluster
ECS_ENGINE_AUTH_DATA={…}  #to pull images from private registries
ECS_AVAILABLE_LOGGING_DRIVERS=[…]   #CloudWatch container logging
ECS_ENABLE_TASK_IAM_ROLE=true       #Enable IAM roles for ECS tasks
```

## ECS Task Definitions

[Tasks definitions](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/task_definitions.html) are metadata in JSON form to tell ECS how to run a Docker Container. It contains crucial information around:
 - Launch Type (fargate or ec2)
 - Image Name
 - Port Binding for Container and Host
 - Memory & CPU
 - Environment variables
 - IAM Role
 - Logging
 - Volume
 - Networking

Task Definition Components:

 - Family: Name and Version
 - Task Role: Role to call AWS Services
 - Task Execution Role: Pull images & publish logs
 - Network Mode: bridge, awsvpc, host
 - Container information: docker run options
 - Volumes: docker volumes & bind mounts
 - Task Placements: Place tasks on ECS instances

[GoTo: Table of Contents](#table-of-contents)

## ECS Service

[ECS Services](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/ecs_services.html) help define how many tasks should run and how they should be run. If any of your tasks should fail or stop for any reason, the ECS service scheduler launches another instance of your task definition to replace it in order to maintain the desired number of tasks in the service.

In addition, you can optionally run your service behind a load balancer across the tasks that are associated with the service.

Topics
 - Service scheduler and Additional service
 - Service Definition Parameters
 - Creating, Updating and Deleting a service
 - Amazon ECS Deployment Types
 - Service Load Balancing
 - Service Auto Scaling
 - Service Discovery
 - Service Throttle Logic
 
[GoTo: Table of Contents](#table-of-contents)

### ECS Service with Load Balancer

ECS service can optionally be configured to use Elastic Load Balancing to distribute traffic evenly across the tasks in your service. ECS services supports the following load balancer types: Application, Network, and Classic. 
 - makes routing decisions at the application layer (HTTP/HTTPS)
 - supports path-based routing, 
 - can route requests to one or more ports on each container instance in your cluster. 

Application Load Balancers support dynamic host port mapping. This dynamic mapping allows you to have multiple tasks from a single service on the same container instance. 

[GoTo: Table of Contents](#table-of-contents)

## ECR

ECR is a private Docker image repository:

 - Access is controlled through IAM (permission errors => policy)
 - AWS CLI v1 login command (may be asked at the exam)
   - `$(aws ecr get-login --no-include-email --region eu-west-1)`
 - AWS CLI v2 login command (newer, may also be asked at the exam - pipe)
   - `aws ecr get-login-password --region eu-west-1 | docker login --username AWS --password-stdin 1234567890.dkr.ecr.eu-west-1.amazonaws.com`
 - Docker Push & Pull:
   - `docker push 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:latest`
   - `docker pull 1234567890.dkr.ecr.eu-west-1.amazonaws.com/demo:latest`


[GoTo: Table of Contents](#table-of-contents)

## Fargate

When launching an ECS Cluster, we have to create our EC2 instances. If we need to scale, we need to add EC2 instances

With Fargate, it’s all Serverless! We don’t provision EC2 instances. We just create task definitions, and AWS will run our containers for us. To scale, just increase the task number. 

[GoTo: Table of Contents](#table-of-contents)

## ECS IAM Roles Deep Dive

EC2 Instance Profile:
 - Used by the ECS agent
 - Makes API calls to ECS service
 - Send container logs to CloudWatch Logs
 - Pull Docker image from ECR

ECS Task Role:
 - Allow each task to have a specific role
 - Use different roles for the different ECS Services you run
 - Task Role is defined in the task definition

[GoTo: Table of Contents](#table-of-contents)

## ECS Tasks Placement

When a task of type EC2 is launched ECS must determine where to place it, with the constraints of CPU,memory, and available port. Similarly, when a service scales in, ECS needs to determine which task to terminate ==> we can define a task placement strategy and task placement constraints

> Note: this is only for ECS with EC2, not for Fargate

**ECS Task Placement Strategies**

ECS Task Placement Process uses the following process to select container instances:
  1. Identify the instances that satisfy the CPU, memory, and port requirements in the task definition.
  2. Identify the instances that satisfy the task placement constraints.
  3. Identify the instances that satisfy the task placement strategies.
  4. Select the instances for task placement.

**Binpack**
 - Place tasks based on the least available amount of CPU or memory
 - This minimizes the number of instances in use (cost savings)

```json
"placementStrategy": [
    {
        "field" : "memory",
        "type" : "binpack"
    }
]
```

**Random**
 - Place the task randomly

```json
"placementStrategy": [{ "type" : "random" }]
```

**Spread**
 - Place the task evenly based on the specified value
 - Example: instanceId, attribute:ecs.availability-zone


```json
"placementStrategy": [
    {
        "field" : "attribute:ecs.availability-zone",
        "type" : "spread"
    },
    {
        "field" : "instanceId",
        "type" : "spread"
    }
]
```

**ECS Task Placement Constraints**

 - distinctInstance
   - place each task on a different container instance
   - `"placementConstraints": [ { "type" : "distinctInstance" } ]`
 - memberOf
   - places task on instances that satisfy an expression
   - uses the Cluster Query Language (advanced)
   - `"placementConstraints": [ { "expression" : "attribute:ecs.availability-zone =~ t2.*", "type" : "memberOf" } ]`


**mixing stuff**
```json
"placementStrategy": [
    {
        "field" : "attribute:ecs.availability-zone",
        "type" : "spread"
    },
    {
        "field" : "memory",
        "type" : "binpack"
    },
    {
        "field" : "instanceId",
        "type" : "spread"
    }
],
"placementConstraints": [
    {
        "expression" : "attribute:ecs.availability-zone =~ t2.*",
        "type" : "memberOf"
    }
]
```

[GoTo: Table of Contents](#table-of-contents)

## ECS Cluster Capacity Provider

A Capacity Provider is used in association with a cluster to determine the infrastructure that a task runs on
 - For ECS and Fargate users, the FARGATE and FARGATE_SPOT capacity providers are added automatically
 - For Amazon ECS on EC2, you need to associate the capacity provider with an auto-scaling group

When you run a task or a service, you define a capacity provider
strategy, to prioritize in which provider to run. This allows the capacity provider to automatically provision
infrastructure for you

[GoTo: Table of Contents](#table-of-contents)


## ECS Summary & Exam Tips

**ECS Classic**
 - EC2 instances must be created
 - We must configure the file /etc/ecs/ecs.config with the cluster name
 - The EC2 instance must run an ECS agent
 - EC2 instances can run multiple containers on the same type:
   - must not specify a host port (only container port)
   - should use an Application Load Balancer with the dynamic port mapping
   - The EC2 instance security group must allow traffic from the ALB on all ports
 - ECS tasks can have IAM Roles to execute actions against AWS
 - Security groups operate at the instance level, not task level

**Fargate**
 - Fargate is Serverless (no EC2 to manage)
 - AWS provisions containers for us and assigns them ENI
 - Fargate containers are provisioned by the container spec (CPU / RAM
 - Fargate tasks can have IAM Roles to execute actions against AWS

 - ECS does integrate with CloudWatch Logs:
   - You need to setup logging at the task definition level
   - Each container will have a different log stream
   - The EC2 Instance Profile needs to have the correct IAM permissions
 - Use IAM Task Roles for your tasks
 - Task Placement Strategies: binpack, random, spread
 - Service Auto Scaling with target tracking, step scaling, or scheduled
 - Cluster Auto Scaling through Capacity Providers


[GoTo: Table of Contents](#table-of-contents)


# AWS Elastic Beanstalk

[AWS Elastic Beanstalk: Setting configuration options before environment creation](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/environment-configuration-methods-before.html)

[AWS Elastic Beanstalk - Deployment policies and settings](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.rolling-version-deploy.html)

## Elastic Beanstalk Overview
Elastic Beanstalk components
 - Application: _application is a logical collection of Elastic Beanstalk components, including environments, versions, and environment configurations._
 - Application version: each deployment gets assigned a version. Application version points to an Amazon Simple Storage Service (Amazon S3) object
 - Environment:
   - name (dev, test, prod…): free naming
   - environment is a collection of AWS resources running an application version
 - Environment tier
   - web server environment tier
   - worker environment tier

You deploy application versions to environments and can promote application versions to the next environment
 - Full control over lifecycle of environments
 - Rollback feature to previous application version

Elastic Beanstalk supports applications developed in
Support for many platforms:
 - Go
 - Java SE
 - Java with Tomcat
 - .NET on Windows Server with IIS
 - Node.js
 - PHP
 - Python
 - Ruby
 - Packer Builder
 - Single Container Docker
 - Multicontainer Docker
 - Preconfigured Docker
 - If not supported, you can write your custom platform (advanced)

## Elastic Beanstalk Deployment Modes

The following list provides summary information about the different [deployment policies](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/using-features.deploy-existing-version.html) and adds related considerations.

**All at once** – The quickest deployment method. Suitable if you can accept a short loss of service, and if quick deployments are important to you. With this method, Elastic Beanstalk deploys the new application version to each instance. Then, the web proxy or application server might need to restart. As a result, your application might be unavailable to users (or have low availability) for a short time.

**Rolling** – Avoids downtime and minimizes reduced availability, at a cost of a longer deployment time. Suitable if you can't accept any period of completely lost service. With this method, your application is deployed to your environment one batch of instances at a time. Most bandwidth is retained throughout the deployment.

**Rolling with additional batch** – Avoids any reduced availability, at a cost of an even longer deployment time compared to the Rolling method. Suitable if you must maintain the same bandwidth throughout the deployment. With this method, Elastic Beanstalk launches an extra batch of instances, then performs a rolling deployment. Launching the extra batch takes time, and ensures that the same bandwidth is retained throughout the deployment.

**Immutable** – A slower deployment method, that ensures your new application version is always deployed to new instances, instead of updating existing instances. It also has the additional advantage of a quick and safe rollback in case the deployment fails. With this method, Elastic Beanstalk performs an immutable update to deploy your application. In an immutable update, a second Auto Scaling group is launched in your environment and the new version serves traffic alongside the old version until the new instances pass health checks.

**Traffic splitting** – A canary testing deployment method. Suitable if you want to test the health of your new application version using a portion of incoming traffic, while keeping the rest of the traffic served by the old application version.


## Elastic Beanstalk CLI

[https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-cmd-commands.html](https://docs.aws.amazon.com/elasticbeanstalk/latest/dg/eb3-cmd-commands.html)

Basic commands are:
 - eb create
 - eb status
 - eb health
 - eb events
 - eb logs
 - eb open
 - eb deploy
 - eb config
 - eb terminate


## Beanstalk Other Stuff

### Beanstalk with HTTPS
 - Idea: Load the SSL certificate onto the Load Balancer
 - Can be done from the Console (EB console, load balancer configuration)
 - Can be done from the code: .ebextensions/securelistener-alb.config
 - SSL Certificate can be provisioned using ACM (AWS Certificate Manager) or CLI
 - Must configure a security group rule to allow incoming port 443 (HTTPS port)


### Beanstalk redirect HTTP to HTTPS
 - Configure your instances to redirect HTTP to HTTPS:
[https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/awsprovided/security-configuration/https-redirect](https://github.com/awsdocs/elastic-beanstalk-samples/tree/master/configuration-files/awsprovided/security-configuration/https-redirect)
 - OR configure the Application Load Balancer (ALB only) with a rule
 - Make sure health checks are not redirected (so they keep giving 200 OK)


[GoTo: Table of Contents](#table-of-contents)

# Dynamodb

[Best Practices for Designing and Architecting with DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)

[Amazon DynamoDB Read/Write Capacity Mode](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/HowItWorks.ReadWriteCapacityMode.html)

[Choosing the Right DynamoDB Partition Key](https://aws.amazon.com/blogs/database/choosing-the-right-dynamodb-partition-key/)

[How to perform ordered data replication between applications by using Amazon DynamoDB Streams](https://aws.amazon.com/blogs/database/how-to-perform-ordered-data-replication-between-applications-by-using-amazon-dynamodb-streams/)

[GoTo: Table of Contents](#table-of-contents)

# AWS CICD - CodeCommit, CodePipeline, CodeBuild, CodeDeploy

**Continuous integration** is a software development practice where members of a team use a version control system and frequently integrate their work to the same location, such as a master branch. Each change is built and verified to detect integration errors as quickly as possible. Continuous integration is focused on automatically building and testing code, as compared to continuous delivery, which automates the entire software release process up to production.

**Continuous delivery** is a software development methodology where the release process is automated. Every software change is automatically built, tested, and deployed to production. Before the final push to production, a person, an automated test, or a business rule decides when the final push should occur. Although every successful software change can be immediately released to production with continuous delivery, not all changes need to be released right away.

Products covered by the exam: 
 - CodeCommit
 - CodePipeline
 - CodeBuild
 - CodeDeploy

**Reference Links for Domain**


## CodeCommit

 - [https://www.atlassian.com/git/tutorials/using-branches](https://www.atlassian.com/git/tutorials/using-branches)
 - [https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control-iam-identity-based-access-control.html](https://docs.aws.amazon.com/codecommit/latest/userguide/auth-and-access-control-iam-identity-based-access-control.html)
 - [https://aws.amazon.com/blogs/devops/refining-access-to-branches-in-aws-codecommit/](https://aws.amazon.com/blogs/devops/refining-access-to-branches-in-aws-codecommit/)
 - [https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-notify.html](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-notify.html)
 - [https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-repository-email.html )](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-repository-email.html ))
 - [https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-notify-lambda.html](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-notify-lambda.html)
 - [https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-migrate-repository-existing.html](https://docs.aws.amazon.com/codecommit/latest/userguide/how-to-migrate-repository-existing.html)


## CodeBuild

 - [https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html](https://docs.aws.amazon.com/codebuild/latest/userguide/build-spec-ref.html)
 - [https://docs.aws.amazon.com/codebuild/latest/userguide/samples.html](https://docs.aws.amazon.com/codebuild/latest/userguide/samples.html)
 - [https://docs.aws.amazon.com/codebuild/latest/userguide/sample-docker.html](https://docs.aws.amazon.com/codebuild/latest/userguide/sample-docker.html)
 - [https://aws.amazon.com/blogs/devops/validating-aws-codecommit-pull-requests-with-aws-codebuild-and-aws-lambda/](https://aws.amazon.com/blogs/devops/validating-aws-codecommit-pull-requests-with-aws-codebuild-and-aws-lambda/)

## CodeDeploy

 - [https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_MinimumHealthyHosts.html](https://docs.aws.amazon.com/codedeploy/latest/APIReference/API_MinimumHealthyHosts.html)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html#appspec-hooks-server](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html#appspec-hooks-server)
 - [https://docs.amazonaws.cn/en_us/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.](https://docs.amazonaws.cn/en_us/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.)html#reference-appspec-file-structure-environment-variable-availability
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/monitoring-cloudwatch-events.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/monitoring-cloudwatch-events.html)
 - [https://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/](https://aws.amazon.com/blogs/devops/view-aws-codedeploy-logs-in-amazon-cloudwatch-console/)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/monitoring-sns-event-notifications.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/monitoring-sns-event-notifications.html)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-rollback-and-redeploy.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployments-rollback-and-redeploy.html)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups-configure-advanced-options.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-groups-configure-advanced-options.html)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-on-premises.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/instances-on-premises.html)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/register-on-premises-instance-iam-user-arn.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/register-on-premises-instance-iam-user-arn.html)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/register-on-premises-instance-iam-session-arn.html](https://docs.aws.amazon.com/codedeploy/latest/userguide/register-on-premises-instance-iam-session-arn.html)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html#deployment-configuration-lambda](https://docs.aws.amazon.com/codedeploy/latest/userguide/deployment-configurations.html#deployment-configuration-lambda)
 - [https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html#appspec-hooks-lambda](https://docs.aws.amazon.com/codedeploy/latest/userguide/reference-appspec-file-structure-hooks.html#appspec-hooks-lambda)


## CodePipeline

 - [https://docs.aws.amazon.com/codepipeline/latest/userguide/reference-pipeline-structure.html#action-requirements](https://docs.aws.amazon.com/codepipeline/latest/userguide/reference-pipeline-structure.html#action-requirements)
 - [https://docs.aws.amazon.com/codepipeline/latest/userguide/best-practices.html#use-cases](https://docs.aws.amazon.com/codepipeline/latest/userguide/best-practices.html#use-cases)
 - [https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-invoke-lambda-function.html](https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-invoke-lambda-function.html)
 - [https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-create-custom-action.html](https://docs.aws.amazon.com/codepipeline/latest/userguide/actions-create-custom-action.html)
 - [https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_PutJobSuccessResult.html](https://docs.aws.amazon.com/codepipeline/latest/APIReference/API_PutJobSuccessResult.html)
 - [https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/continuous-delivery-codepipeline.html](https://docs.aws.amazon.com/AWSCloudFormation/latest/UserGuide/continuous-delivery-codepipeline.html)
 - [https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-cloudformation.html](https://docs.aws.amazon.com/codepipeline/latest/userguide/tutorials-cloudformation.html)
 - [https://github.com/aws-samples/codepipeline-nested-cfn](https://github.com/aws-samples/codepipeline-nested-cfn)
 - [https://aws.amazon.com/blogs/devops/implementing-gitflow-using-aws-codepipeline-aws-codecommit-aws-codebuild-and-aws-codedeploy/](https://aws.amazon.com/blogs/devops/implementing-gitflow-using-aws-codepipeline-aws-codecommit-aws-codebuild-and-aws-codedeploy/)


## CodeStar

 - [https://docs.aws.amazon.com/codestar/latest/userguide/templates.html](https://docs.aws.amazon.com/codestar/latest/userguide/templates.html)


## Jenkins

 - [https://aws.amazon.com/getting-started/projects/setup-jenkins-build-server/](https://aws.amazon.com/getting-started/projects/setup-jenkins-build-server/)
 - [https://wiki.jenkins.io/display/JENKINS/Amazon+EC2+Plugin](https://wiki.jenkins.io/display/JENKINS/Amazon+EC2+Plugin)
 - [https://aws.amazon.com/blogs/devops/simplify-your-jenkins-builds-with-aws-codebuild/](https://aws.amazon.com/blogs/devops/simplify-your-jenkins-builds-with-aws-codebuild/)
 - [https://wiki.jenkins.io/display/JENKINS/AWS+CodeBuild+Plugin](https://wiki.jenkins.io/display/JENKINS/AWS+CodeBuild+Plugin)
 - [https://wiki.jenkins.io/display/JENKINS/Amazon+EC2+Container+Service+Plugin](https://wiki.jenkins.io/display/JENKINS/Amazon+EC2+Container+Service+Plugin)


