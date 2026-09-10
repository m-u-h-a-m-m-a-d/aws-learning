# SNS Use Cases - Comprehensive Reference

## Quick Use Case Matrix

| Use Case | Architecture | Key Services | Complexity | ROI |
|----------|--------------|--------------|------------|-----|
| Push Notifications | A2P | SNS + Mobile Platform | Low | High |
| System Alerts | A2A | SNS + CloudWatch | Low | High |
| Process Coordination | A2A | SNS + Lambda + SQS | Medium | High |
| Parallel Processing | A2A | SNS + Multi-services | Medium | Very High |
| Email Campaigns | A2P | SNS + Email | Low | High |
| Payment Notifications | A2A | SNS + Payment Gateway | Medium | High |
| Cross-Account | A2A | SNS + IAM | Medium | Medium |

---

## 1. PUSH NOTIFICATIONS

### Overview
Deliver real-time notifications directly to mobile devices

### Architecture
```
Mobile App Event → SNS Topic → Mobile Push Platform → User Device
```

### AWS Services Involved
- Amazon SNS – Message distribution
- Amazon SNS Platform Application Endpoint – Mobile routing
- AWS Device Farm – Testing (optional)

### Implementation Steps
1. Create SNS Topic for push notifications
2. Create Platform Application (iOS/Android)
3. Register app on mobile devices
4. Subscribe devices to topic
5. Publish notification messages

### Code Example (Pseudocode)
```
sns.publish(
  TopicArn: 'arn:aws:sns:region:account:MobileNotifications',
  Message: 'Your order has shipped!',
  MessageAttributes: {
    AWS.SNS.MOBILE.priority: 'high',
    AWS.SNS.MOBILE.TTL: '3600'
  }
)
```

### Real-World Scenarios
- **E-commerce:** Order confirmation, shipping update, delivery notification
- **Food Delivery:** Order accepted, driver arriving, order ready
- **Social Media:** New message, comment mention, friend request
- **Finance:** Account alert, balance low, unusual activity
- **Health:** Appointment reminder, medication time, test results

### Benefits
- Direct user engagement
- High delivery rates (95%+)
- Real-time notification
- Cross-platform support
- Rich media support

### Costs
- $0.50 per million mobile push notifications (AWS Free Tier: 1M/month)
- No charge for SNS publish requests to mobile platform
- Mobile SDK integration may vary by platform

### Best Practices
- Use campaign analytics to track engagement
- Segment users by interests/preferences
- Avoid excessive notifications (fatigue)
- Include rich media when relevant
- Test on real devices before launch
- Implement opt-in/opt-out mechanisms

---

## 2. SYSTEM ALERTS

### Overview
Distribute system and infrastructure alerts to operations teams

### Architecture
```
Monitoring System → SNS Topic → Email/SMS/Slack → Operations Team
```

### AWS Services Involved
- Amazon CloudWatch – Metrics and alarms
- Amazon SNS – Alert distribution
- AWS Lambda – Alert enrichment (optional)
- Amazon SQS – Alert queuing (optional)

### Implementation Steps
1. Set up CloudWatch alarms for key metrics
2. Configure SNS topic as alarm action
3. Subscribe operations channels (email, SMS)
4. Test alert flow
5. Set up escalation policies

### Integration Points
```
CPU Usage High → CloudWatch Alarm
              ↓
          SNS Topic
              ↓
         ┌─────┴──────┬──────────┐
         ↓            ↓          ↓
      Email        SMS        Lambda
         ↓            ↓          ↓
      Engineer   Operations  Auto-Remediation
```

### Alert Types to Monitor
- **Infrastructure:** CPU, memory, disk, network
- **Application:** Error rate, response time, exceptions
- **Database:** Connection pool, query latency, replication lag
- **Cost:** Budget alerts, cost anomalies
- **Security:** IAM changes, unauthorized access

### Real-World Scenarios
- Database CPU exceeds 80% → Alert ops team
- Application error rate > 1% → Page on-call engineer
- Disk usage > 85% → Auto-trigger cleanup
- Unusual API activity → Security team notified
- High AWS costs → Finance team alerted

### Benefits
- Immediate incident detection
- Reduces MTTR (Mean Time To Resolution)
- Multi-channel notification
- Scalable to many metrics
- Audit trail of alerts

### Costs
- SNS publish: $0.50 per million (mostly free tier)
- CloudWatch alarms: $0.10 each per month
- SMS charges: ~$0.50-1.00 per message

### Best Practices
- Set appropriate alarm thresholds
- Avoid alert fatigue (too many false positives)
- Use multiple notification channels for critical alerts
- Implement escalation rules
- Document alert response procedures
- Regular alert tuning
- Post-incident review of alert effectiveness

---

## 3. PROCESS COORDINATION

### Overview
Coordinate actions across multiple services for distributed workflows

### Architecture
```
Initial Event → SNS Topic ├→ Service A
                         ├→ Service B
                         └→ Service C (all in parallel)
```

### AWS Services Involved
- Amazon SNS – Event distribution
- AWS Lambda – Event handlers
- Amazon SQS – Queuing
- AWS Step Functions – Orchestration (complex workflows)
- Amazon DynamoDB – State tracking

### Implementation Steps
1. Define workflow steps and their dependencies
2. Create SNS topic for workflow events
3. Implement event handlers (Lambda/SQS consumers)
4. Add state management (DynamoDB)
5. Implement error handling and retries

### Workflow Examples

**Order Processing:**
```
OrderPlaced Event → SNS
                  ├→ Process Payment (Lambda)
                  ├→ Reserve Inventory (Lambda)
                  ├→ Notify Warehouse (SQS)
                  ├→ Log Analytics (Firehose)
                  └→ Send Confirmation (Email)
```

**Data Pipeline:**
```
DataUpload Event → SNS
                 ├→ Validate Data (Lambda)
                 ├→ Transform Data (Lambda)
                 ├→ Load to Warehouse (Firehose)
                 └→ Update Dashboard (Lambda)
```

### Real-World Scenarios
- **E-commerce Order:** Payment → Inventory → Fulfillment → Notification
- **User Onboarding:** Registration → Send Welcome → Create Profile → Setup Permissions
- **Document Processing:** Upload → Scan → Recognize → Store → Index
- **Fraud Detection:** Transaction → Risk Score → Decision → Action

### Benefits
- Loose coupling between services
- Parallel processing of independent tasks
- Easy to add/remove workflow steps
- Scales to many steps and services
- Resilient to individual step failures

### Costs
- SNS: $0.50 per million publishes
- Lambda: $0.20 per million invocations
- SQS: $0.40 per million requests
- Step Functions: $25 per million state transitions

### Best Practices
- Design for failure (implement retries)
- Use idempotent operations
- Implement distributed transactions
- Track workflow state
- Log all workflow events
- Monitor workflow performance
- Set timeouts on all steps

---

## 4. PARALLEL PROCESSING

### Overview
Process same message across multiple subscribers simultaneously

### Architecture
```
Message → SNS Topic ├→ Processor 1 (independent)
                   ├→ Processor 2 (independent)
                   └→ Processor N (independent)
```

### AWS Services Involved
- Amazon SNS – Message distribution
- AWS Lambda – Parallel processors
- Amazon SQS – Output queues
- Amazon S3 – Results storage

### Implementation Steps
1. Create SNS topic for work items
2. Subscribe multiple Lambda functions or SQS queues
3. Each subscriber processes independently
4. Aggregate results (if needed)

### Processing Examples

**Image Processing:**
```
ImageUpload → SNS
           ├→ Lambda: Resize
           ├→ Lambda: Filter
           ├→ Lambda: Tag (ML)
           └→ Lambda: Watermark
           
Results → S3 (all variants stored)
```

**Report Generation:**
```
ReportRequest → SNS
              ├→ Lambda: PDF Report
              ├→ Lambda: Excel Report
              ├→ Lambda: Email Summary
              └→ Lambda: Archive
              
Results → S3 (all formats available)
```

### Real-World Scenarios
- **Media Processing:** Image resizing, transcoding, thumbnail generation
- **Document Processing:** PDF conversion, OCR, text extraction, indexing
- **Data Transformation:** Multiple format exports in parallel
- **Analytics:** Real-time updates to multiple dashboards

### Benefits
- Reduced total processing time
- Efficient resource utilization
- Fault isolation (one failure doesn't stop others)
- Scales easily (add more subscribers)
- Simple architecture

### Costs
- SNS publish: $0.50 per million
- Lambda invocations: $0.20 per million (×number of processors)
- Storage: S3 costs for results

### Best Practices
- Make processors independent
- Implement timeout handling
- Monitor individual processor performance
- Use Lambda reserved concurrency
- Aggregate results separately
- Handle partial failures gracefully
- Log all processor activity

---

## 5. MOBILE NOTIFICATIONS

### Overview
Engage mobile app users with targeted, personalized notifications

### Architecture
```
User Event → SNS Topic → Mobile Push Service → User's Phone
```

### AWS Services Involved
- Amazon SNS – Message hub
- AWS Pinpoint – Advanced campaign management
- Amazon SNS Platform Application – Mobile routing
- AWS Lambda – Personalization

### Implementation Steps
1. Set up AWS Pinpoint or SNS Platform Application
2. Configure push notification settings
3. Create user segments
4. Publish targeted messages
5. Track delivery and engagement

### Notification Types
- **Informational:** Status updates, news alerts
- **Promotional:** Sales, new features, discounts
- **Transactional:** Confirmations, receipts, alerts
- **Engagement:** Re-engagement, personalized recommendations

### Real-World Scenarios
- **Ride-Sharing:** "Driver is 5 minutes away"
- **E-commerce:** "Your order is ready for pickup"
- **Streaming:** "New episode of your favorite show available"
- **Banking:** "Unusual login detected"
- **Social:** "Your friend liked your post"

### Segmentation Examples
- By geography (region, city)
- By behavior (purchase history, app usage)
- By demographics (age, device)
- By engagement (active, inactive)

### Benefits
- Direct user engagement
- Personalization at scale
- Rich analytics available
- A/B testing capabilities
- Multi-device delivery

### Costs
- AWS Free Tier: 1M mobile push/month
- Beyond free tier: $0.50 per million
- Additional costs if using Pinpoint

### Best Practices
- Segment users by interests
- Personalize based on user behavior
- Respect notification preferences
- A/B test message content
- Monitor engagement metrics
- Avoid notification fatigue
- Include clear call-to-action

---

## 6. CROSS-ACCOUNT COMMUNICATION

### Overview
Enable secure communication between different AWS accounts

### Architecture
```
Account A (Publisher) → SNS Topic
                     ↓
              Cross-Account Access
                     ↓
Account B (Subscriber) → Lambda/Application
```

### AWS Services Involved
- Amazon SNS – Cross-account topic
- AWS IAM – Access policies
- AWS KMS – Encryption (optional)

### Permission Setup

**Topic Policy (in Account A):**
```json
{
  "Effect": "Allow",
  "Principal": {
    "AWS": "arn:aws:iam::ACCOUNT-B:root"
  },
  "Action": "sns:Subscribe",
  "Resource": "arn:aws:sns:region:ACCOUNT-A:TopicName"
}
```

**Subscriber IAM Policy (in Account B):**
```json
{
  "Effect": "Allow",
  "Action": "sns:Subscribe",
  "Resource": "arn:aws:sns:region:ACCOUNT-A:TopicName"
}
```

### Implementation Steps
1. Create topic in Account A
2. Add cross-account policy to topic
3. Create IAM role in Account B
4. Subscribe from Account B to Account A topic
5. Test end-to-end communication

### Real-World Scenarios
- **Multi-Tenant SaaS:** Tenant events to shared infrastructure
- **Organizational Structure:** Subsidiary events to parent company
- **Partner Integration:** Partner events to your systems
- **Centralized Logging:** All accounts log to central account
- **Shared Services:** Multiple teams sharing infrastructure

### Organizational Structures
```
Company Hub (Account A)
├─ Division 1 (Account B) → subscribes
├─ Division 2 (Account C) → subscribes
└─ Division 3 (Account D) → subscribes

All divisions notified of company-wide events
```

### Benefits
- Secure integration between accounts
- No credential sharing required
- Centralized management
- Audit trail via CloudTrail
- Cost optimization via shared infrastructure

### Costs
- Cross-account doesn't add charges
- Standard SNS pricing applies

### Best Practices
- Use least privilege principle
- Encrypt sensitive messages (KMS)
- Audit access regularly
- Use role assumption for automation
- Document cross-account relationships
- Test failover scenarios
- Monitor cross-account activity

---

## 7. AUTOMATED EMAIL MARKETING CAMPAIGNS

### Overview
Trigger and send email campaigns based on user actions or schedules

### Architecture
```
User Action/Schedule → SNS Topic → Email Service → Customer Inbox
```

### AWS Services Involved
- Amazon SNS – Email delivery
- AWS SES (Simple Email Service) – Email sending
- AWS Lambda – Campaign logic
- Amazon DynamoDB – User preferences
- Amazon Pinpoint – Advanced campaigns

### Campaign Types

**Transactional:**
- Order confirmation
- Password reset
- Account verification
- Receipt/invoice

**Behavioral:**
- Welcome email (signup)
- Cart abandonment (browsing)
- Re-engagement (inactive)
- Post-purchase follow-up

**Promotional:**
- Flash sale announcement
- New product launch
- Seasonal promotion
- Personalized offers

**Automated Workflows:**
- Drip campaigns (sequence over time)
- Triggered flows (event-based)
- Preference-based (customer selected)

### Implementation Steps
1. Set up SNS topic for emails
2. Configure SES as email provider
3. Create campaign logic (Lambda)
4. Store user preferences (DynamoDB)
5. Trigger campaigns (events or schedule)
6. Track delivery and engagement

### Real-World Scenarios
- **E-commerce:**
  - Welcome series (3 emails over week)
  - Abandoned cart (1 hour after abandon)
  - Post-purchase (5 days after delivery)
  
- **SaaS:**
  - Onboarding series (daily for 7 days)
  - Feature announcement (to opted-in users)
  - Usage-based upsell (based on behavior)

- **Content Platform:**
  - New article notification
  - Weekly digest
  - Author follow notifications

### Personalization
- Use customer name
- Reference purchase history
- Tailor content to interests
- Dynamic pricing based on segment
- Personalized recommendations

### Benefits
- Automated, scalable delivery
- Triggered by real events
- Highly personalized
- Compliance with email regulations
- Cost-effective
- Detailed analytics

### Costs
- SNS: Minimal (publishing cost)
- SES: $0.10 per thousand emails

### Best Practices
- Always include unsubscribe link
- Honor user preferences
- Comply with CAN-SPAM Act
- Use double opt-in
- Monitor bounce rates
- A/B test subject lines
- Segment by engagement level
- Monitor unsubscribe rates
- Provide value in every email
- Test on multiple clients

---

## 8. PAYMENT PROCESSING NOTIFICATIONS

### Overview
Notify relevant systems and users when payment status changes

### Architecture
```
Payment Event → SNS Topic ├→ Customer Email
                         ├→ Inventory System
                         ├→ Fulfillment
                         ├→ Analytics
                         └→ Accounting
```

### AWS Services Involved
- Amazon SNS – Event distribution
- Payment Gateway integration
- AWS Lambda – Event processing
- Amazon SQS – Queue processing
- Amazon DynamoDB – State tracking

### Payment Events

**Successful Payments:**
- Payment authorized
- Payment captured
- Subscription renewed
- Refund processed

**Failed Payments:**
- Declined
- Expired card
- Insufficient funds
- Network error

**Suspicious Payments:**
- Fraud detection triggered
- Multiple failed attempts
- Unusual amount
- Unusual location

### Notification Recipients

**Customer:**
- Confirmation email/SMS
- Receipt/invoice
- Refund confirmation
- Account updated

**Internal Systems:**
- Inventory → Update stock
- Fulfillment → Start process
- Analytics → Record transaction
- Accounting → Record revenue

**Management:**
- Failed payment alert
- High volume notification
- Fraud alert
- Revenue dashboard update

### Implementation Steps
1. Integrate payment gateway with SNS
2. Create SNS topic for payment events
3. Subscribe systems to events
4. Implement event handlers (Lambda/SQS)
5. Send customer notifications
6. Track payment status

### Real-World Scenarios
- **E-commerce:**
  - Payment success → Order confirmation + Fulfillment
  - Payment failure → Retry notification + Alternative payment

- **Subscription Service:**
  - Payment success → Renewal confirmation + Access restored
  - Payment failure → Retry attempt + Account suspension warning

- **Marketplace:**
  - Seller receives payment → Payout notification
  - Buyer refund processed → Refund confirmation + Return initiated

### Error Handling
- Payment timeout → Retry logic
- Failed payment → Notify customer with alternative methods
- Duplicate payment → Refund and investigation
- Partial payment → Request additional payment

### Benefits
- Real-time payment status updates
- Multiple systems synchronized
- Immediate customer notification
- Audit trail of all transactions
- Automated reconciliation
- Fraud detection integration

### Costs
- SNS publish: ~$0.50 per million
- Lambda processing: Per invocation
- Additional storage/processing as needed

### Best Practices
- Never store credit card details
- Use PCI-compliant payment processors
- Implement retry logic with exponential backoff
- Encrypt sensitive payment data
- Monitor for fraud patterns
- Maintain audit trail
- Test payment flows thoroughly
- Handle partial refunds correctly
- Implement reconciliation logic

---

## Choosing the Right Use Case

**Ask these questions:**

1. **Who are the recipients?**
   - End users → Push notifications, email
   - Internal systems → Event coordination
   - Operations team → System alerts

2. **How time-sensitive is it?**
   - Real-time critical → Direct notification
   - Can wait → Queue-based processing
   - Scheduled → Step Functions

3. **How many subscribers?**
   - One → Direct message
   - Multiple → Fan-out
   - Many services → Parallel processing

4. **Complexity level?**
   - Simple → SNS only
   - Medium → SNS + Lambda
   - Complex → SNS + Step Functions + multiple services

5. **Accuracy requirements?**
   - Best-effort OK → Standard topic
   - Must be exact order → FIFO topic
   - Can't lose messages → Add SQS

---

**Last Updated:** September 11, 2026  
**Course:** Amazon SNS, Getting Started  
**Lesson:** 2 - Architecture and Use Cases
