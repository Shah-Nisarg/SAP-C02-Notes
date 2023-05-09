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
