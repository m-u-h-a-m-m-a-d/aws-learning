# Amazon SNS: Architecture and Use Cases - Lesson 2

**Course:** Amazon SNS, Getting Started  
**Lesson:** 2 - Architecture and Use Cases (Lesson 3 of 8)  
**Date:** September 10, 2026

---

## 📚 Lesson Objectives

In this lesson, you will learn how to do the following:

- Recognize the relationship between Amazon SNS and other components of the AWS Cloud
- Recognize the meaning of Amazon SNS technical concepts
- Identify typical use cases for Amazon SNS

---

## How is Amazon SNS Used to Architect a Cloud Solution?

With Amazon SNS, you can build robust, scalable, and efficient cloud solutions. By integrating Amazon SNS with various AWS services, you can create sophisticated architectures for real-time communication, event-driven processing, and automated workflows.

### Two Main Architecture Patterns

Amazon SNS supports two primary architectural patterns:

#### 1. Application-to-Person (A2P)

**Purpose:** Send notifications from applications to end users

**Components:**
- **Publisher** – Application or service that generates messages
- **SNS Topic** – Central hub for organizing messages
- **Subscribers** – User endpoints that receive notifications

**Supported Endpoints:**
- Email addresses
- Mobile phone numbers (SMS)
- Mobile push notifications
- HTTP/HTTPS webhooks

**Use Case Example:** E-commerce order confirmation system sends notifications to customers via email and SMS simultaneously

**Benefits:**
- Reach users through their preferred communication channel
- Single API call broadcasts to all subscribers
- Automatic retry and delivery guarantee

---

#### 2. Application-to-Application (A2A)

**Purpose:** Enable seamless communication between AWS services and custom applications

**Components:**
- **Publisher** – Application, microservice, or AWS service generating events
- **SNS Topic** – Event distribution hub
- **Subscribers** – AWS services and applications that process events

**Supported Subscribers:**
- **AWS Lambda** – Trigger serverless functions
- **Amazon SQS** – Queue messages for batch processing
- **Amazon Data Firehose** – Stream data to analytics services
- **Amazon EC2** – HTTP servers via HTTP/HTTPS endpoints
- **Amazon S3** – Store objects or trigger workflows
- **Amazon Redshift** – Load data into data warehouse
- **Other AWS services** – Custom integrations

**Use Case Example:** E-commerce order system publishes events that trigger:
- Lambda to calculate shipping
- SQS to queue fulfillment tasks
- S3 to archive order data
- Redshift to record analytics

**Benefits:**
- Decoupled architecture (services don't need direct integration)
- Event-driven processing in real-time
- Scalable handling of high message volumes
- Supports complex multi-step workflows

---

## What AWS Services Integrate with Amazon SNS?

Amazon SNS provides seamless integration with many AWS services, enabling event-driven architectures and automated workflows.

### AWS Lambda

**Integration:** SNS can directly trigger Lambda functions when messages are published to a topic

**How It Works:**
- Message published to SNS topic
- SNS automatically invokes subscribed Lambda function
- Lambda processes the message and returns result
- Enables serverless, event-driven architectures

**Common Use Cases:**
- Image processing on S3 upload
- Email sending on user signup
- Database record processing
- Real-time data transformation

---

### Amazon SQS

**Integration:** SNS can publish messages to SQS queues for decoupled processing

**How It Works:**
- Messages published to SNS topic
- Automatically forwarded to subscribed SQS queues
- Applications retrieve messages from queue at their own pace
- Provides message buffering and durable storage

**Common Use Cases:**
- Batch processing of events
- Decoupling frontend from backend services
- Handling traffic spikes with queue buffering
- Ensuring no message loss during processing

**Key Difference:**
- SNS = Push-based delivery (sends messages to subscribers)
- SQS = Pull-based retrieval (subscribers retrieve messages)

---

### Amazon CloudWatch

**Integration:** SNS can send notifications based on CloudWatch alarms

**How It Works:**
- CloudWatch monitors metrics (CPU, memory, errors, etc.)
- When alarm threshold is breached, CloudWatch triggers SNS
- SNS delivers notification to subscribed endpoints (email, SMS, Lambda)

**Common Use Cases:**
- System performance alerts
- Infrastructure monitoring notifications
- Cost anomaly alerts
- Application error alerts

---

### Amazon S3

**Integration:** S3 can notify SNS when events occur in buckets

**How It Works:**
- S3 bucket configured to send notifications to SNS
- Events triggered: object creation, deletion, etc.
- SNS distributes notifications to subscribers

**Common Use Cases:**
- Trigger processing on file upload
- Notify applications of bucket changes
- Archive objects to Glacier
- Start data pipeline on new data arrival

---

### AWS Auto Scaling

**Integration:** SNS notifies when Auto Scaling events occur

**How It Works:**
- Auto Scaling Group triggers scaling events
- SNS notifies subscribers about scaling actions
- Can trigger Lambda or other actions based on scaling

**Common Use Cases:**
- Notify teams of infrastructure changes
- Trigger cleanup operations on scale-down
- Alert operations team of unusual scaling
- Adjust resources in dependent services

---

### Amazon DynamoDB

**Integration:** DynamoDB Streams can trigger SNS notifications

**How It Works:**
- DynamoDB table changes captured in stream
- Can trigger Lambda to publish to SNS
- Enables event-driven responses to data changes

**Common Use Cases:**
- Notify when important data changes
- Update cache on data modification
- Trigger data synchronization
- Audit critical data changes

---

### AWS CloudFormation

**Integration:** SNS notifies on CloudFormation stack events

**How It Works:**
- Stack creation, update, or deletion triggers SNS
- Notifications sent when lifecycle events occur
- Can trigger Lambda for custom automation

**Common Use Cases:**
- Notify team of infrastructure provisioning
- Trigger validation after stack creation
- Alert on stack failures
- Automate post-deployment tasks

---

### Amazon Data Firehose

**Integration:** SNS can send messages to Firehose for stream processing

**How It Works:**
- SNS publishes messages to Firehose delivery stream
- Firehose buffers and delivers to destination
- Destinations: S3, Redshift, Elasticsearch, Splunk

**Common Use Cases:**
- Stream real-time event data to data warehouse
- Enable data analytics on SNS messages
- Create data pipelines
- Archive event data for analysis

---

## What Are the Basic Technical Concepts of Amazon SNS?

Understanding SNS technical concepts is essential for effective implementation.

### Topics

**Definition:** A topic is a logical access point and communication channel that decouples message publishers from subscribers.

**How It Works:**
- Publishers send messages to a topic (not directly to subscribers)
- All subscribers to that topic receive the same messages
- Topic acts as a hub for message distribution

**Topic Types:**

**Standard Topics:**
- Default topic type for most scenarios
- Good for applications that can handle messages arriving more than once and out of order
- Support maximum throughput
- Use at-least-once delivery semantics
- Best-effort ordering (generally in order, but not guaranteed)
- Suitable when duplicate messages are acceptable

**FIFO Topics (First-In-First-Out):**
- Designed for strict ordering and deduplication requirements
- Messages delivered exactly once (no duplicates)
- Messages delivered in exact order received
- Essential when operation order is critical
- Lower throughput compared to Standard topics
- Use cases: financial transactions, inventory management, order processing

---

### Publishers and Subscribers

**Publisher:**
- An entity (application, service, Lambda function, etc.) that sends messages to an SNS topic
- Publishers don't need to know subscriber details
- Creates loose coupling in architecture
- Can be:
  - Applications (web, mobile, desktop)
  - AWS services (S3, CloudWatch, DynamoDB)
  - Custom microservices
  - Lambda functions

**Subscriber:**
- An entity that receives messages from an SNS topic
- Must subscribe to a topic before receiving messages
- Can be:
  - Email addresses
  - SMS phone numbers
  - Lambda functions
  - SQS queues
  - HTTP/HTTPS endpoints
  - AWS services (Firehose, Data Pipeline)
  - Custom applications

---

### Subscriptions

**Definition:** A subscription maps a topic to a subscriber endpoint.

**How It Works:**
- Before receiving messages, subscribers must subscribe to a topic
- Subscription creates a connection between topic and endpoint
- Multiple subscriptions can exist for a single topic
- Multiple topics can have the same subscriber

**Subscription Lifecycle:**
1. Subscriber requests subscription to topic
2. SNS creates subscription and assigns subscription ARN
3. Depending on protocol, may require confirmation (e.g., email)
4. Once active, subscriber begins receiving messages
5. Subscriber can unsubscribe at any time

**Subscription Protocols:**
- HTTP/HTTPS – Custom web applications
- Email – Direct user notification
- SMS – Mobile phone text message
- SQS – Amazon queue service
- Lambda – Serverless function invocation
- Platform Application Endpoint – Mobile app push notifications

---

### Message Delivery

**How SNS Delivers Messages:**

1. **Message Reception** – Publisher sends message to SNS topic
2. **Message Storage** – SNS temporarily stores message
3. **Subscriber Lookup** – SNS identifies all active subscriptions
4. **Delivery Attempt** – SNS attempts to deliver to each subscriber endpoint
5. **Protocol-Specific Delivery:**
   - HTTP(S): POST request to webhook URL
   - Email: Formatted email message
   - SMS: Text message to phone number
   - Lambda: Direct function invocation
   - SQS: Message placed in queue

**Delivery Guarantees:**
- **At-Least-Once Delivery** – Each message delivered to each subscriber at least once
- **Best-Effort Ordering** – Messages generally delivered in order sent, but not guaranteed
- **Automatic Retry** – Failed deliveries automatically retried with exponential backoff
- **Dead-Letter Queue** – After max retries, messages can be sent to DLQ (for SQS subscriptions)

---

### Fan-Out Pattern

**Definition:** The fan-out pattern allows a single message to be delivered to multiple subscribers simultaneously.

**How It Works:**
1. Publisher sends one message to SNS topic
2. SNS distributes message to all subscribed endpoints
3. Multiple different endpoints receive same message in parallel
4. Enables asynchronous, event-driven processing

**Benefits:**
- Decouples publisher from subscribers
- Enables parallel processing
- Scales to many subscribers without publisher changes
- Asynchronous, non-blocking communication

**Example Scenario:**
```
Order placed → SNS Topic
              ├→ Lambda (payment processing)
              ├→ SQS (warehouse fulfillment)
              ├→ Email (customer notification)
              ├→ S3 (audit logging)
              └→ DynamoDB (analytics)
```

**Real-World Use Case:**
E-commerce order creation triggers 5 different systems in parallel without the order service needing to know about them.

---

### Message Filtering

**Definition:** Message filtering allows subscribers to define rules to receive only specific messages from a topic.

**How It Works:**
- Subscribers define filter policies (JSON format)
- Filter policies specify message attributes or content patterns
- Only messages matching the filter are delivered to subscriber
- Reduces unnecessary message delivery
- Saves costs by avoiding delivery of irrelevant messages

**Filter Policy Example:**
```json
{
  "order_type": ["express", "priority"],
  "region": ["US-West", "US-East"]
}
```

This policy delivers only messages with these specific order types and regions.

**Benefits:**
- Reduces message processing overhead
- Subscriber receives only relevant messages
- Enables selective event processing
- Decreases costs for message-based pricing

**Use Cases:**
- Geo-specific notifications
- Event type filtering
- Conditional processing
- Resource optimization

---

### Encryption with AWS KMS

**Definition:** Server-side encryption (SSE) protects message contents using AWS Key Management Service (AWS KMS).

**How It Works:**
- SNS encrypts messages at rest using AWS KMS keys
- Keys managed in AWS Key Management Service
- Automatic encryption/decryption transparent to applications
- Each message encrypted individually

**Encryption Levels:**
1. **Managed Encryption:** AWS manages keys (default)
   - Simple setup
   - No key management overhead
   - Suitable for most applications

2. **Customer-Managed Keys:** Organization manages keys
   - Fine-grained access control
   - Key rotation policies
   - Audit trail of key usage
   - Required for compliance frameworks

**Security Benefits:**
- Messages protected at rest
- Prevents unauthorized access to message content
- Maintains compliance with data protection regulations
- Works seamlessly with subscription types (SQS, Lambda, etc.)

**Considerations:**
- Additional cost for customer-managed keys
- Key management responsibility
- Must grant SNS permission to use keys

---

## What Are Typical Use Cases for Amazon SNS?

Amazon SNS enables scalable, event-driven architectures. Here are common use cases:

### Push Notifications

**What It Is:** Deliver targeted notifications to mobile devices and applications

**How It Works:**
- Application publishes notification message to SNS topic
- SNS routes to mobile platforms (iOS, Android, etc.)
- Users receive push notification on their device
- Lightweight and non-intrusive user engagement

**Real-World Examples:**
- Social media new message alerts
- E-commerce order status updates
- Mobile game achievement notifications
- Banking transaction alerts
- News app breaking news alerts

**Benefits:**
- Direct user engagement
- Real-time delivery
- High delivery rates
- Cross-platform support

---

### System Alerts

**What It Is:** Distribute alerts about infrastructure issues or system events

**How It Works:**
- Monitoring system (CloudWatch, custom) detects issue
- Publishes alert message to SNS
- Alert delivered to operations team via email, SMS, or app
- Enables quick response to critical issues

**Real-World Examples:**
- Database down alert
- CPU utilization exceeding threshold
- Disk space running low
- Application error rate spike
- Security vulnerability detected

**Benefits:**
- Immediate team notification
- Multiple notification channels
- Escalation capabilities
- Audit trail of alerts

---

### Process Coordination

**What It Is:** Coordinate actions across distributed systems for workflow completion

**How It Works:**
- First service completes task and publishes event to SNS
- Multiple services subscribe to event
- Each service performs its coordinated action
- Services work together without direct integration

**Real-World Examples:**
- Distributed transaction coordination
- Microservice workflow orchestration
- Multi-stage approval processes
- Complex business workflows
- Batch job orchestration

**Benefits:**
- Services remain loosely coupled
- Complex workflows become manageable
- Easy to add/remove workflow steps
- Scales to many services

---

### Parallel Processing

**What It Is:** Publish messages to multiple subscribers for simultaneous processing

**How It Works:**
- Single message published to topic
- Multiple subscribers process in parallel
- Each subscriber performs independent work
- Results combined or aggregated separately

**Real-World Examples:**
- Image processing (resize, filter, analyze in parallel)
- Report generation (multiple formats generated simultaneously)
- Data replication to multiple databases
- Document processing (OCR, translation, storage in parallel)
- Analytics updates to multiple systems

**Benefits:**
- Reduced total processing time
- Efficient resource utilization
- Scalable to many parallel processors
- Independent failure handling

---

### Mobile Notifications

**What It Is:** Engage mobile app users with targeted notifications

**How It Works:**
- App event triggers notification
- Message published to SNS
- SNS formats for mobile platforms
- Delivered to user's phone/tablet
- User sees notification on lock screen or app

**Real-World Examples:**
- Ride-sharing pickup notification
- Delivery tracking update
- Sale/promotion alerts
- Sports score notifications
- Chat message notification

**Benefits:**
- Increases app engagement
- Keeps users informed
- Higher open rates vs. email
- Rich notification formatting support

---

### Cross-Account Communication

**What It Is:** Enable secure communication between AWS accounts

**How It Works:**
- Topic in Account A publishes to SNS
- Services in Account B subscribe to Account A topic
- SNS handles access control and authentication
- Cross-account integration without shared credentials

**Real-World Examples:**
- Multi-tenant SaaS platform
- Shared infrastructure notifications
- Partner integrations
- Organizational cross-team communication
- Subsidiary company coordination

**Benefits:**
- Secure cross-account integration
- No credential sharing required
- Granular access control via IAM
- Audit trail of cross-account access

---

### Automated Email Marketing Campaigns

**What It Is:** Send targeted email campaigns triggered by user events

**How It Works:**
- User action triggers event (signup, purchase, etc.)
- Event published to SNS
- SNS sends email to user list
- Personalized content included
- Unsubscribe and preference management available

**Real-World Examples:**
- Welcome email on signup
- Cart abandonment reminders
- Order confirmation emails
- Promotional campaign emails
- Re-engagement emails for inactive users

**Benefits:**
- Automated, scalable email delivery
- Triggered by real events
- Personalization capabilities
- Compliance with email regulations
- Cost-effective bulk sending

---

### Payment Processing Notifications

**What It Is:** Notify systems and users about payment status changes

**How It Works:**
- Payment gateway processes transaction
- Status change published to SNS
- Multiple subscribers receive notification:
  - Customer receives confirmation/alert
  - Inventory system updates stock
  - Fulfillment system starts processing
  - Analytics system records transaction
  - Accounting system records revenue

**Real-World Examples:**
- Payment success confirmation to customer
- Payment failure alert to customer
- Refund processed notification
- Subscription renewed confirmation
- Fraudulent transaction alert

**Benefits:**
- Real-time payment status updates
- Multiple systems synchronized
- Customer transparency
- Reduced manual reconciliation

---

## What Else Should I Keep in Mind About Amazon SNS?

### Integration with Existing Systems and Business Processes

**Challenge:** SNS must integrate with legacy systems and existing workflows

**Considerations:**
- Custom subscriptions may be needed (HTTP/HTTPS webhooks)
- Event source development required
- Event handler development needed
- Workflow automation may require AWS Step Functions or third-party tools

**Best Practices:**
- Plan integration points carefully
- Use adapters/translators if formats don't match
- Test end-to-end workflows
- Document event schemas

---

### Design for High Availability and Scalability

**Challenge:** SNS must handle growing message volumes and maintain service availability

**Considerations:**
- Use multiple topics if needed for logical separation
- Combine SNS with SQS for buffering during spikes
- Distribute workloads across multiple Regions/Availability Zones
- Handle duplicate messages in subscriber logic
- Implement idempotent processing

**Best Practices:**
- Use FIFO topics when strict ordering needed
- Implement exponential backoff in subscribers
- Monitor delivery metrics
- Use Dead-Letter Queues for failed messages

---

### Implement Cost Tracking and Budget Optimization

**Challenge:** SNS costs can grow with scale, especially with high message volume

**Considerations:**
- Monitor requests, data transfer, and retries
- Use message filtering to reduce unnecessary deliveries
- Implement lifecycle policies
- Consider Reserved Instances for predictable workloads

**Cost Optimization Strategies:**
- Batch messages when possible
- Use message filtering
- Combine SNS with SQS for efficient buffering
- Monitor and analyze costs with AWS Cost Explorer
- Set AWS Budgets for cost alerts

**Cost Tracking Tools:**
- AWS Budgets for budget management
- AWS Cost Explorer for analysis
- CloudWatch metrics for monitoring

---

### Integrate with AWS Security Services

**Challenge:** Secure SNS against unauthorized access and data breaches

**Security Measures:**

**Access Control:**
- Use IAM policies to control topic access
- Implement resource-based policies on topics
- Restrict who can publish/subscribe

**Data Protection:**
- Enable encryption using AWS KMS
- Encrypt data in transit (TLS/SSL)
- Use VPC endpoints for private connectivity

**Monitoring and Audit:**
- Enable CloudTrail for API call logging
- Use CloudWatch for metrics and alarms
- Set up SNS access logging
- Monitor suspicious activity

**Best Practices:**
- Follow principle of least privilege
- Regularly review permissions
- Rotate encryption keys
- Enable MFA for sensitive operations

---

## Quiz Review: Key Concepts

### Question 1: Multi-Protocol Delivery

**Question:** Which Amazon SNS feature sends messages to multiple recipient protocols simultaneously?

**Correct Answer:** Fan-out

**Explanation:** The fan-out pattern is a key SNS feature that allows a message published to a topic to be delivered to multiple endpoint types (email, SMS, HTTP, Lambda, SQS) simultaneously.

---

### Question 2: AWS Service Integration

**Question:** Which AWS service can Amazon SNS directly integrate with to trigger automated actions based on published messages?

**Correct Answer:** AWS Lambda

**Explanation:** Amazon SNS can directly integrate with AWS Lambda, allowing Lambda functions to be automatically invoked when messages are published to an SNS topic. This enables serverless, event-driven architectures.

While EC2, RDS, and EBS can interact with SNS, Lambda is the primary service for direct automated triggering.

---

### Question 3: Common Use Case

**Question:** Which is a common use case for Amazon SNS?

**Correct Answer:** Real-time event notification

**Explanation:** Amazon SNS is designed specifically for sending real-time notifications and messages, making it ideal for event-driven architectures and alerting systems.

Long-term data storage, serverless computing (that's Lambda), and content delivery acceleration (that's CloudFront) are not primary SNS use cases.

---

## Key Takeaways

1. **Two Main Architectures:**
   - A2P (Application-to-Person) for user notifications
   - A2A (Application-to-Application) for service communication

2. **Wide AWS Integration:** SNS integrates with Lambda, SQS, S3, CloudWatch, Firehose, and many other services

3. **Technical Concepts:**
   - Topics are communication hubs
   - Fan-out enables parallel processing
   - Filtering reduces unnecessary messages
   - Encryption protects sensitive data

4. **Versatile Use Cases:**
   - Push notifications for mobile apps
   - System alerts and monitoring
   - Event-driven workflow coordination
   - Cross-account communication
   - Marketing automation

5. **Important Considerations:**
   - Plan integration carefully
   - Design for scale and availability
   - Monitor costs closely
   - Implement security best practices

---

## Next Steps

- Study architecture patterns in detail
- Practice creating topics and subscriptions
- Explore AWS service integrations
- Build sample event-driven applications
- Review security and compliance requirements

---

**Course Status:** Lesson 2 Complete ✅  
**Next:** Lesson 3 - Getting Started with SNS (Hands-on)
