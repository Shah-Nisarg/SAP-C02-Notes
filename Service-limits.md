Compute
=

EC2
-

Savings Plan
-

Reserved Instances
-

AMI
-

Spot instances
-

Lambda
-

ECS
-

ECR
-

EKS
-

API Gateway
-

AppSync
-

Route 53
-

Global Accelerator
-

CloudFront
-

Storage
=

S3 Buckets
-

S3 Access Points
-

S3 Multi-region access points
-

S3 Object Lambda
-

EFS
-

EBS
-

FSx
-

Storage gateway
-

Snowball family
-

DataSync
-

ElastiCache
-

Database
=

DynamoDB
-
|                             | Category                | Quota                                |                             |
|-----------------------------|-------------------------|--------------------------------------|-----------------------------|
| Per table                   | On demand               | **40k RCU, 40k WCU**                 | May be less in some regions |
|                             | Provisioned             | **40k RCU, 40k WCU**                 |                             |
| Per account                 |                         | **80k RCU, 80k WCU**                 |                             |
| Table size                  |                         | No limit                             |                             |
| Table count                 | Per account, Per region | 2500 default 10,000 hard limit       |                             |
| Local Secondary Indexes     | Per table               | Max. 5                               |                             |
| Global Secondary Indexes    | Per table               | Max. 20                              |                             |
| Projected attributes        | Per secondary index     | Max. 100                             |                             |
| Partition Key               | Length                  | 1-2048 bytes                         |                             |
| Sort Key                    | Length                  | 1-1024 bytes                         |                             |
| Partition Size              |                         | 10 GB                                |                             |
| **Item Size**               | Names + Values          | **_400 KB_**                         | ⚠️                         |
| Item Size + LSI values size |                         | **_400 KB_**                         |                             |
| Transactions                | unique items            | 100                                  | Only 1 account and 1 region |
|                             | data volume             | 4 MB                                 |                             |
| Streams                     | Consumers               | 2 if single region 1 if global table |                             |



RDS
-

Aurora
-

OpenSearch
-

Document DB
-

Data Engineering
=

Kinesis Data Streams
-

Kinesis Data Firehose
-

Kinesis Data Analytics
-

Kinesis Video Streams
-

MSK
-

Batch
-

EMR
-

Glue
-

Redshift
-

Athena
-

Quicksight
-

Data Pipeline
-

Data Sync
-

Database migration service
-

Networking
=

VPC
-

Peering
-

Transit gateway
-

DX
-

VIF
-

PrivateLink
-

Gateway endpoint
-

Interface endpoint
-

S2S VPN
-

Network Firewall
-


Security
=

CloudTrail
-

KMS
-

Parameter Store
-

Secrets Manager
-

ACM
-

Cloud HSM
-

Shield
-

WAF
-

GuardDuty
-

Inspector
-

Config
-

Firewall Manager
-

Detective
-

Identity
=

IAM Roles
-

IAM Users
-

IAM Groups
-

STS
-

Organizations
-

Directory Service
-

Control Tower
-

RAM - Resource Access Manager
-

SCP
-

Monitoring
= 

CloudWatch
-

EventBridge
-

X-Ray
-

Deployment
=

Cloud Formation
-

CF Stack sets
-

Service catalog
-

SAM
-

CDK
-

SSM
-

Cost Management
=

Trusted advisor
-



Others
=

SQS
-

SNS
-

MQ
-


FIS
-

AWS Backup
-

VM Migration Services
-
