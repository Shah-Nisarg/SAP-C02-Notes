Fundamentals
=

- Users - long term credentials
- Roles - short term credentials
  - Using STS
  - Service roles
- JSON policy
  - Statement
  - Effect
  - Principal
  - Action
  - Resource
  - Condition
  - ```
    {
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    }
    ```
- ⚠️ Access Advisor - which roles are in use, what permissions, when was the last time, etc.
- ⚠️ **Access Analyzer** - Permissions granted to external entity
  - Static analysis of IAM roles and resource policies
  - Analysis based on zone of trust
  - ‼️ Can use cloud trail to identify the exact permissions required for a role and generate a policy
- Policy variables:
  - `${aws.username}`
  - `${aws.sourceIp}`
  - `${iam:ResourceTag/Key}` or `${aws:PrincipalTag/Key}`
  - `${s3:prefix}`
  - more...
- Cross role access
  - Options:
    - Assume role in target account
    - Resource policy allows access (Preferred) ✅
- Permission boundary
  - Limit what a user or a role can do
  - Similar to SCP (Service Control Policy) at organization OU/Account level
- Policy evaluation logic
  - https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html

STS
=

- Generates temporary credentials (15 mins to 12 hours)
- Revoke sessions
  - Disallow sessions based on timestamp (RevokeOlderSessions)
- APIs
  - AssumeRole
    - ‼️ Securing 3rd party access to AssumeRole
      - An **External ID** is generated to establish trust between two accounts.
      - When an account assumes role, it passes the external ID to the AssumeRole API call.
      - The role is assumed only if the External ID is matching.
    - AssumeRoleWithSAML - for users logged in using SAML (AD, ADFS, AWS SSO)
      - Must establish trust
    - AssumeRoleWithWebIdentity - for users logged in using IdP
      - Google, Facebook, Cognito
    - GetSssionToken - for MFA
    - GetFederationToken - for a federated user ❓❓❓
      - Used when the identity provider doesn't have support for SAML or WebIDP. In such a case, the identity broker calls AssumeRole or GetFederationToken directly to generate AWS credentials. 
- Session Tags
  - The user calling assumeRole API may pass a session tag (e.g. Department = HR) and resource policy may use policy variables such as `${aws:PrincipalTag/Department} = "HR"` to grant fine-grained permissions to the role based on session tag.
  
Directory Services
=

- Domain Controller and Machines
- ADFS - provides single sign on across applications
- Directory options
  - AWS Managed Microsoft AD
    - Independent AD
    - Can establish trust with on-prem
      - through DX or VPN
      - trust != replication
      - ⚠️ Users remain separate, but applications can authenticate them in either AD
    - within VPC
    - Integrations - RDS SQL Server, AWS workspaces, Quicksight, AWS SSO
    - Multi-AZ with multiple domain controller
    - Multi-region replication supported
  - AD Connector
    - proxy to on-prem
    - Users are present only on-prem
    - No caching
    - No support for SQL server
  - Simple AD
    - AD-compatible
    - No trust with Microsoft AD
    - 5000 users 
    - No MFA, no SQL server, no AWS SSO
    - Low cost 💲
- ⚠️ Replication
  - On-prem AD can replicate to an AD controller hosted in EC2
  - Custom implemenation (i.e. not managed by AWS)

Organizations
= 

- Root -> OU -> Account
- OrganizationAccountAccessRole - management account can assume the role in member account
  - Must be created manually for new account joining the organization
- Tagging standards for billing
- CloudTrail across accounts sending logs to one S3 bucket
- CloudWatch logs to one account
- ⚠️ Organization features (pick one)
  - Consolidated billing
    - Aggregated usage and billing
    - Single payment method
    - Reserved instance benefits (can be disabled for individual accounts)
  - All features (default)
    - Consolidated billing
    - SCP
    - Prevent members from leaving
    - ‼️ Note: The member account must approve "All features"
    - Can't go back
- SCP
  - Don't apply to
    - root account
    - ⚠️ service linked roles (you can't restrict AWS services)
  - Do apply to
    - root users of member accounts
  - ⚠️ Deny everything by default
  - Applies to OU or account
- Tag Policies
  - Consistent tag keys and vlaues
- AI service opt-out policy
  - Prevent AWS from using your data for improving their services
- Backup policies
  - View only backup plan that applies to the account
  - 
