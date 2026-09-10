# SNS Technical Concepts - Quick Reference

## Core Concepts at a Glance

### Topic
- **Logical access point** and communication channel
- **Decouples publishers from subscribers**
- Acts as a hub for message distribution
- Two types: Standard and FIFO

### Publisher
- Sends messages to SNS topic
- Doesn't need to know subscriber details
- Can be: applications, AWS services, Lambda, microservices

### Subscriber
- Receives messages from topic
- Must subscribe before receiving messages
- Can be: email, SMS, Lambda, SQS, HTTP, custom apps

### Subscription
- Maps a topic to a subscriber endpoint
- Requires activation (some types need confirmation)
- Multiple subscriptions per topic
- Protocols: HTTP/HTTPS, Email, SMS, SQS, Lambda

---

## Message Delivery Flow

```
1. Publisher publishes message to SNS Topic
   ↓
2. SNS stores message temporarily
   ↓
3. SNS identifies all active subscriptions
   ↓
4. SNS attempts delivery to each subscriber
   ↓
5. Protocol-specific delivery:
   - HTTP: POST request
   - Email: Formatted email
   - SMS: Text message
   - Lambda: Function invocation
   - SQS: Queue message
   ↓
6. Delivery confirmation or automatic retry
```

---

## Topic Types Comparison

| Feature | Standard | FIFO |
|---------|----------|------|
| **Ordering** | Best-effort | Guaranteed FIFO |
| **Duplicates** | May occur | Exactly-once |
| **Throughput** | Maximum | Lower |
| **Use Case** | General purpose | Critical ordering |
| **Example** | Notifications | Financial transactions |

---

## Subscriber Protocol Comparison

| Protocol | Best For | Confirmation | Cost |
|----------|----------|--------------|------|
| **Email** | User alerts | Yes | Low |
| **SMS** | Urgent alerts | Yes | Medium |
| **HTTP/HTTPS** | Custom apps | No | Low |
| **Lambda** | Automation | No | Per invocation |
| **SQS** | Buffering | No | Per message |
| **Push Notification** | Mobile apps | Yes | Per notification |

---

## Fan-Out Pattern

**What:** One message to many subscribers simultaneously

**Benefits:**
- Parallel processing
- Decoupled architecture
- Asynchronous communication
- Scalable to many subscribers

**Example:**
```
Order Event → SNS
           ├→ Payment Processor
           ├→ Inventory System
           ├→ Warehouse
           ├→ Analytics
           └→ Customer Email
```

---

## Message Filtering

**Purpose:** Subscribers receive only specific messages

**Filter Format:** JSON policy on message attributes

**Benefits:**
- Reduces message overhead
- Lowers costs
- Focused processing
- Resource optimization

**Example:**
```json
{
  "order_status": ["shipped", "delivered"],
  "priority": ["high", "urgent"],
  "region": ["US"]
}
```

---

## Encryption with AWS KMS

**What:** Server-Side Encryption protects message contents

**Types:**
- **AWS-Managed** – AWS handles keys
- **Customer-Managed** – You manage keys

**Protection:**
- Messages at rest encrypted
- Transparent to applications
- Compliance-ready

---

## Message Delivery Guarantees

| Guarantee | Details |
|-----------|---------|
| **At-Least-Once** | Each message delivered to each subscriber minimum once |
| **Best-Effort Ordering** | Generally in order, but not guaranteed |
| **Automatic Retry** | Failed deliveries retried with backoff |
| **Dead-Letter Queue** | After max retries, send to DLQ (SQS) |

---

## Common Integration Patterns

### Lambda Trigger
```
Event → SNS → Lambda Function → Action
```
- Serverless processing
- Event-driven
- No polling needed

### SQS Buffering
```
Event → SNS → SQS Queue → Consumer
```
- Decoupling
- Load balancing
- Fault tolerance

### Multi-Destination
```
Event → SNS → Lambda
            → SQS
            → Email
            → Webhook
```
- Parallel processing
- Multiple actions
- Flexible architecture

---

## Naming Convention Best Practices

**Topic Names:**
- Use descriptive names: `OrderNotifications`, `SystemAlerts`
- Use prefixes: `prod-`, `staging-`, `dev-`
- Example: `prod-order-events`, `staging-user-alerts`

**Subscription Names:**
- Indicate endpoint type: `OrderNotifications-Email`, `OrderNotifications-Lambda`
- Include service: `OrderNotifications-InventorySQS`

---

## Publisher Best Practices

| Practice | Reason |
|----------|--------|
| Use message attributes | Enable filtering |
| Include message IDs | Track messages |
| Use JSON format | Parse easily |
| Set TTL appropriately | Manage message lifespan |
| Error handling | Retry logic needed |

---

## Subscriber Best Practices

| Practice | Reason |
|----------|--------|
| Idempotent processing | Handle duplicates |
| Validate messages | Prevent errors |
| Use filter policies | Reduce overhead |
| Implement DLQ | Capture failures |
| Log all events | Audit trail |

---

## Cost Considerations

**Pricing Components:**
- **Requests** – Per publish request
- **Data Transfer** – Out of region
- **Retries** – Failed delivery attempts
- **Push Notifications** – Per mobile notification

**Cost Optimization:**
- Use message filtering
- Batch messages
- Use FIFO only if needed
- Monitor CloudWatch metrics
- Set up budget alerts

---

## Security Checklist

- [ ] Enable encryption (KMS)
- [ ] Restrict IAM policies
- [ ] Use resource-based policies
- [ ] Enable CloudTrail logging
- [ ] Monitor CloudWatch metrics
- [ ] Implement MFA
- [ ] Use VPC endpoints
- [ ] Regular permission audit
- [ ] Encrypt data in transit
- [ ] Handle sensitive data carefully

---

## Common Error Scenarios

| Error | Cause | Solution |
|-------|-------|----------|
| **InvalidParameter** | Bad topic ARN | Verify topic exists and ARN correct |
| **NotFound** | Topic doesn't exist | Create topic first |
| **AuthorizationError** | Insufficient permissions | Update IAM policy |
| **InvalidAttributeValue** | Invalid message attribute | Check attribute format |
| **ThrottlingException** | Rate limit exceeded | Implement backoff logic |

---

## Monitoring Key Metrics

**CloudWatch Metrics to Watch:**
- `NumberOfMessagesPublished` – Message volume
- `NumberOfNotificationsFailed` – Delivery failures
- `NumberOfNotificationsDelivered` – Successful deliveries
- `PublishSize` – Average message size

---

## Troubleshooting Guide

**Messages Not Received:**
1. Verify subscription is confirmed
2. Check IAM permissions
3. Review filter policies
4. Check CloudWatch logs
5. Verify endpoint is reachable

**High Costs:**
1. Review message volume
2. Check data transfer patterns
3. Implement message filtering
4. Use SQS for buffering
5. Analyze retry patterns

**Delivery Delays:**
1. Check endpoint availability
2. Review CloudWatch metrics
3. Check for throttling
4. Verify network connectivity
5. Monitor Lambda duration

---

**Last Updated:** September 11, 2026  
**Course:** Amazon SNS, Getting Started  
**Lesson:** 2 - Architecture and Use Cases
