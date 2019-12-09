## SVS320-R - [REPEAT] The serverless journey of shop.LEGO.com

Connecting the LEGO play experience with millions of people requires an innovative platform. This has fueled the cloud migration of the legacy e-commerce application. In this session, we walk you through the principles, the approach, the learnings, and of course the serverless technologies that made the vision a reality. We cover multiple real-world use cases such as the integration of the e-commerce platform with the tax system, and the implementation of an event-streaming platform.

### How it all started
- Monolith
- Oracle ATG, SAP, and Tax Platform
- Then Elastic Beantalk container with Node.js + React
- Then added VPC gateway
- Black Friday 2017 - System went down
    - Slowness in Tax system led to many queued requests, leading to 503s for a couple of hours.
- SaaS tax system was introduced, utilizing API gateway as interface with Lambda 
- Black Friday 2018 - no issues

### A journey through patterns
- Atomic request-response API
    - ![](./Photos/SVS320/IMG_1909.JPG)
    - Use Case: Add item to shopping basket
- Command Query Responsibility Segregation (CQRS) with status cache
    - ![](./Photos/SVS320/IMG_1910.JPG)
    - Use Case: Status polling for long-running processes
- Email notification with signed URL
    - ![](./Photos/SVS320/IMG_1913.JPG)
    - Use Case: Voucher codes generation and notification
- API authorizer with identity lookup
    - ![](./Photos/SVS320/IMG_1916.JPG)
    - Use Case: User identity lookup in different systems (legacy, merger, etc.)
- Publish-subscribe sync
    - ![](./Photos/SVS320/IMG_1919.JPG)
    - Use Case: On-demand customer data migration
        - ![](./Photos/SVS320/IMG_1917.JPG)
- Event-driven data pipeline with buffering
    - ![](./Photos/SVS320/IMG_1921.JPG)
    - Use Case: Product catalog import and update
    - Works for multiple object types
        - - ![](./Photos/SVS320/IMG_1922.JPG)
- Codeless data ingestion
    - ![](./Photos/SVS320/IMG_1926.JPG)
    - Use Case: API-driven data ingestion
- Codeless sequence generator
    - ![](./Photos/SVS320/IMG_1927.JPG)
    - Use Case: Unique order ID generation
    - DynamoDB Atomic Counter
        - ![](./Photos/SVS320/IMG_1928.JPG)
- URL redirects cached by CDN
    - ![](./Photos/SVS320/IMG_1929.JPG)
    - Use Case: Website migration with URL changes
- Scheduled workflow
    - ![](./Photos/SVS320/IMG_1930.JPG)
    - Use Case: Keeping website sitemaps updated
- Hub and spoke event bus
    - ![](./Photos/SVS320/IMG_1932.JPG)
    - - ![](./Photos/SVS320/IMG_1933.JPG)
    - Use Case: Checkout event processing
        - ![](./Photos/SVS320/IMG_1931.JPG)

### Takeaways
- Look for something simple to begin with
- Implement automated integration tests
    - Don't spend time mocking AWS amazon services
    - Separate dev, test, and prod accounts
- Architect in "set pieces"
- No throwaway PoCs
- Leverage patterns