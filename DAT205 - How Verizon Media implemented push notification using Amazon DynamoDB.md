## DAT205 - How Verizon Media implemented push notification using Amazon DynamoDB

Verizon Media had to create a better, stronger, and faster push notification system to serve the requirements of iconic Verizon brands, fulfill push notification completion time of 27 million devices in under three minutes, and consistently show the push "toast" on all users’ lock screens. Verizon decided to use Amazon DynamoDB and other AWS services such as Amazon ElastiCache and Amazon SQS in conjunction with its own Vespa search engine to power all the use cases of its brands. It also uses Kubernetes to orchestrate microservices across many Amazon EC2 instances. Join this session to learn how Verizon has been able to accomplish all of this.

### Overview of DynamoDB
- Internet-scale apps: Massive user, data volume, and requests
- NoSQL databases scales much better horizontally
- Fully managed, high performance, enterprise ready
- New version of DynamoDB Global Tables
    - Convert your existing single-region tables to Global tables
    - Add new AWS regions to existing Global Tables
    - Faster and less expensive replication
- DynamoDB Streams
    - Build ordered, near real-time data processing capabilities

### Technical requirements for Verizon Notification Service
- Push to 100k's of devices
- Minimize idle resources and don't over provision
- Asynchronous event-based architeture to achieve parallelism
- 24/7/365

### Architecture
- Registration, association, and subscription APIs
- Push notification