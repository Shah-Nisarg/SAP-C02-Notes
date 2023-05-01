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

Polly
=

- Text to speech
- Standard or Neural
- Uses SSML

Reckognition
=

- Image and Video analysis
- Objects, people, text, face, activities
- 💲 per minute or per image pricing

Textract
= 

- Machine learning product
- OCR
- Output - text, structure, analysis
- Usually real time
- 💲per usage, with custom pricing for large volume of usage

Transcribe
=

- Speech to text
- Full text indexing of audio
- Meeting notes

Translate
=

- Text translation using ML
- Auto-detect language

Amazon Forecast
=

- Forecast time series data
- Retail demand, server capacity, web traffic, etc.

Amazon Fraud Detector
=

- Identify fraud in historical data
- new accounts, payments, guest checkout
- Transaction fraud (credit card)
- Phishing or social media attack

Sage Maker
=

- Fully managed machine learning service
- Fetch, clean, prepare, train, evaluate, deploy, monitor
- Studio for development of models
- 💲 Only costs for the resources it creates (containers, storage)
