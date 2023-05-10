CloudTrail
=

- History of API calls
- Trail can apply to all regions or one region
- Management events 
  - Control plane events
  - e.g. S3 bucket creation
- Data events
  - Not logged by default
  - e.g. S3 object level activity
  - e.g. lambda invocation
- Cloudtrail Insights
  - detect unusual activity
- Stored for 90 days by default
  - Send to S3 or CloudWatch for longer
  - Analyze using Athena
- ‼️ EventBridge can get event for each API call
- Log file integrity validation for S3
  - SHA-256 hashing and signing
- Organization trail = common trail in the management account ✅
- How to react to events:
  - EventBridge (fastest)
  - CloudWatch Logs (through streaming, metric filter)
  - S3 (Athena etc.)

KMS
=

- Encryption
- Symmetric
  - 4 KB data encryption
- Asymmetric
  - ex. sign/verify, encryption outside KMS
- ✅ CMK
  - you manage rotation, deletion, key policy
  - available for envelope encryption
- ⚠️ AWS Managed Key
  - automatically rotated every 1 year
  - view key policy
  - audit in cloud trail
  - No management by you
- 🔒 AWS Owned Key
  - used by AWS for multiple accounts
  - No audit
  - No management by you
  - Unknown rotation policy
- Key Material
  - KMS
    - Creates and manages the key material
    - ⚠️ FIPS Level 2
  - External ⚠️⚠️⚠️⚠️⚠️
    - You can import key material and secure it
    - Must be symmetric key
      - ⛔ Cannot bring asymmetric key
    - Must be rotated manually
    - ⛔ Cannot be used with Cloud HSM
  - **Cloud HSM**
    - Your hardware containing the key
    - Key doesn't leave cloud HSM
    - Very secure
    - ✅ FIPS level 3
    - Support symmetric and asymmetric encryption
    - 🤯 Must use **Cloud HSM client software**, no AWS APIs
      - 🔒 Separate access control
      - ⛔ No IAM
    - If you lose CloudHSM device, the key is lost
    - HA
      - CloudHSM cluster = multi-AZ
    - ‼️ It can do SSL offloading (i.e. decrypting HTTPS traffic) on EC2 instances having NGINX or IIS
- Multi-region Keys
  - Same key in multiple regions
  - Same key ID in multiple regions
  - One replica can become a primary key
  - Disaster recovery
  - Distributed signing

Parameter Store
=

- 4 KB to 8 KB
- Version tracking
- Integration with cloud formation
- Encryption support with KMS
- Hierarchical data store
- 🤯🤯🤯 Allow access to secrets manager: `/aws/reference/secretsmanager/secret_ID_in_secrets_manager`
- ✅ Allow access to public AMIs: `/aws/service/ami-amazon-linux-latest/amzn2-ami-hvm-x86_64-gp2` for every region
- ⚠️ Parameter policies can be used to apply TTL (to expire old passwords)

Secrets Manager
= 

- Rotation of secrets
- Integration with RDS, redshipt, document DB
- Resource based policy
- Encryption using KMS

RDS
=

- Security at rest
  - TDE (transparent data encryption) for Oracle and SQL Server
- Security in transit
  - SSL is supported for all DBs
- IAM authentication is supported by PostGreSQL, MySQL and MariaDB
- Autorization always happens in RDS
- EBS volumes/snapshots can be encrypted using KMS

ACM
=

- Free
- Automatically renews every year
- Integrates with CloudFront, ELB (ALB and NLB), API gateway
- SNI (Server name indication) 
  - loading multiple certificates on a single server or ELB
  - certificate is returned based on the host name
- DNSSEC - ensure that the DNS routes are secure and validated using hash
- Provides SSL termination at ALB ✅
- Supports private certificates
- ⚠️ Manually uploaded certificates must be renewed manually
- ⚠️⚠️ Certificates can only be associated to resources within that region
- ⚠️⚠️⚠️ CloudFront certificates are required in _us-east-1_

S3
=

Encryption methods:
1. SSE-S3
2. SSE-KMS
  -  Keys from KMS
  -  Audit using cloud trail
  -  Requires **GenerateDataKey** permission
4. SSE-C
  - Must use **HTTPS**
6. Client side encryption
7. Glacier - always encrypted using AES-256

Enforcing HTTPS:
- Bucket policy with SecureTransport

Other features:
-

- Access Logs
- Event notifications
  - May skip notifications if versioning is not enabled
- Event Bridge
- IAM policy
- Bucket policy (resource based)
- ACL (Bucket and Object)
- Pre-signed URL 
  - (download or upload) 🤯
  - The URL inherits the permission of IAM identity that created URL
  - if the identity loses permissions, the URL also loses permissions
  - if the identity expires (such as temporary credentials of a role), the URL becomes unauthorized
- VPC Endpoint (Gateway (free) and Interface)
  - Policy for VPC endpoint (`AWS:SourceIp`, `AWS:SourceVPCe`, `AWS:SourceVPC`)
- Object Lock
  - WORM
  - Governance mode or compliance mode
- Glacier vault lock
  - Need to confirm lock in 24 hours
  - Lock cannot be removed once applied
  - Apply a custom policy
- Access Points
  - Separate access point policies for different groups of users
  - Different URLs (`s3://finance-ap.s3.amazonaws.com`)
  - Can be exposed to internet or VPC
    - Requires a VPC endpoint for VPC access point
    - Requires a separate VPC endpoint access policy
- Multi-region access points
  - 1 access point, multiple buckets
  - Bi-directional Cross-region replication
  - Failover control (Active-active, Active-passive)
  - Routes traffic to nearest bucket
  - Replication time control (15 mins and metrics) ‼️
  - Must enable versioning
- Object Lambda
  - Change lambda function to replace the object before returning it
  - e.g. redact data, enrich data with a different DB
  - has a separate access point

Shield
=

- Shield standard - DDoS protection basic - default, free
- Advanced - cost per organization 🏛️
  - insurance against increased cost
  - 24 * 7 support
  - metrics
- CloudFront, Route 53, ALB

WAF
=

- ALB, CloudFront, API Gateway, AppSync (GraphQL)
- Layer 7 protection
- OWASP
- ⚠️ Block IP, country, block by size, block by rate, block bots
- ✅ Action - Count, allow, block, captcha
- Example: CloudFront in front of ALB can inject a custom header that WAF (on ALB) will use to filter request using Web ACL. This prevents unauthorized traffic from hitting the ALB.
- 🏛️ **Firewall manager** can be used to apply consistent WAF rules across accounts ‼️✅

Firewall manager
= 

- Rules in 🏛️ all accounts in organization
  - WAF
  - Shield advanced
  - Security groups
  - Network Firewall at VPC
  - Route 53 resolver DNS firewall
- Rules are region specific
- Rules are automatically applied to new resources ‼️

Inspector
=

- Security assessment on **running** EC2, containers (ECR) and Lambda functions
- uses SSM agent
- network and OS
- Send findings to ✅ Security hub and event bridge

GuardDuty
=

- ML to analyze various events in AWS account
- Data
  - CloudTrail (management and data events)
  - VPC flow logs
  - DNS logs
  - K8s audit logs
- Notify event bridge
- 🤯 Detects crypto currency mining 
- 🏛️ For central management - make an account a delegated administrator in the organization 

Config
=

- Rules to check AWS resource compliance
- History
- ⛔ Cannot prevent API actions
- ✅ Allows remediation using **SSM automation** or lambda
- Periodic or in response to API
- per region service
- Can aggregate into one account 🏛️

Logs
=

- ELB access logs - S3
- S3 access logs - S3
- CloudTrail - S3, CW
- VPC Flow logs - S3, CW, KDF
- Route 53 access logs - CW
- CF access logs - S3
- Config - S3

How to block an IP
=

- NACL on subnet that has
  - EC2
  - Or, ALB
  - Or, EC2 if it has NLB in front
- WAF on ALB with IP filtering
- WAF on CloudFront
