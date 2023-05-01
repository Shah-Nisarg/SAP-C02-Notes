Lex
=

- Text or voice conversational interface
- Powers **Alexa**
- Speech recognition
- Natural language understanding - intent recognition
- 💲 Pay as you go, serverless
- Lambda integration

Connect
=

- Contact center as a service
- Voice, Chat, incoming, outgoing
- Traditional and Cellular network
- Agents can connect from anywhere using internet
- 💲 Pay as you go, serverless
- No contact center required
- Can **Connect** cellular call to **Lex**, and respond using Lambda functions
  - Like Twilio

Kinesis Video Streams
=

- Ingest live video streams
- Security cameras, smartphones, cars, drones, etc.
- Integrates with **Reckognition** and Connect
- Output to Kinesis data stream, lambda for analysis
- ⚠️ No direct integration with S3, EBS etc.

Glue
=

- 💲 Serverless ETL
  - vs **Data Pipeline** EMR uses servers
- Source: S3, RDS, JDBC DB, Redshift, DynamoDB, Kinesis, Kafka
- Target: S3, RDS, JDBC DB

Glue data catalog
=

- Metadata about data in region
- One catalog per region per account
- Crawlers can be configured to find data and populate the catalog
- Catalog is used by Athena, Redshift Spectrum, EMR, etc.

Device Farm
=

- Managed web and mobile application testing
  - like browser stack?
- real devices
- Supports remote connection to devices
- Testing hundreds of configurations/combinations on devices

Comprehend
=

- Understand documents
- Natural language processing
- Entities, phrases, language, PII, sentiments

Kendra
=

- Intelligent search service
- Factoid questions - who, what, where
- Descriptive questions - how
- Index - for organizing data
- Data source - s3, confluence, RDS, Web crawler, etc.
