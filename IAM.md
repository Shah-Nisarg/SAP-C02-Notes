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
    - AssumeRoleWithSAML - for users logged in using SAML
    - AssumeRoleWithWebIdentity - for users logged in using IdP
      - Google, Facebook, Cognito
    - GetSssionToken - for MFA
    - GetFederationToken - for a federated user ❓❓❓
- Session Tags
  - The user calling assumeRole API may pass a session tag (e.g. Department = HR) and resource policy may use policy variables such as `${aws:PrincipalTag/Department} = "HR"` to grant fine-grained permissions to the role based on session tag.
  - 
