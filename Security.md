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
  - Cloud HSM
    - Your hardware containing the key
    - Key doesn't leave cloud HSM
    - Very secure
    - ✅ FIPS level 3
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

