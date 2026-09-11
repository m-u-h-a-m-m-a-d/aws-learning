# Creating an Amazon SNS Topic - Lesson 3

**Course:** Amazon SNS, Getting Started  
**Lesson:** 3 - Creating an Amazon SNS Topic (Lesson 4 of 8)  
**Type:** Hands-On Practical  
**Date:** September 11, 2026

---

## 📚 Lesson Objectives

In this lesson, you will learn how to:

- Create an Amazon SNS topic using the AWS Management Console
- Understand topic naming conventions and best practices
- Choose between Standard and FIFO topic types
- Configure topic properties and display names
- Verify successful topic creation

---

## Overview: Creating an SNS Topic

An SNS topic is the fundamental building block of Amazon SNS. Creating a topic is the first step in using Amazon SNS for messaging and notification services.

**Why Topics Matter:**
- Topics act as communication channels
- Topics decouple publishers from subscribers
- Topics enable message distribution to multiple endpoints
- All SNS operations revolve around topics

**Creating a topic is simple but understanding the options is important for proper configuration.**

---

## Step-by-Step Guide: Creating a Topic in AWS Console

### Step 1: Access the AWS Management Console

1. Open your web browser
2. Navigate to the AWS Management Console
3. Sign in with your AWS credentials
4. You're now on the AWS Console dashboard

### Step 2: Navigate to Amazon SNS

**Method 1: Using the Search Bar (Recommended)**
1. Look for the search bar at the top of the AWS Console
2. Type "SNS" in the search bar
3. Results will appear as you type
4. Click on "Simple Notification Service" from the results

**Method 2: Using the Services Menu**
1. Click on "Services" (top-left corner)
2. Find "Messaging" or "Application Integration" section
3. Click on "Simple Notification Service"

**Method 3: Using Favorites**
1. If SNS is a favorite, click on it directly from the dashboard

### Step 3: Choose "Simple Notification Service"

Once you search for SNS, you'll see the SNS service option. Click on it to open the SNS console.

**What You'll See:**
- SNS Dashboard
- Left sidebar with options (Topics, Subscriptions, Dead-letter queues, etc.)
- Main content area showing existing topics (if any)

### Step 4: Create a New Topic

1. Look for a button labeled **"Create topic"** (usually blue/purple button in top-right area)
2. Click the "Create topic" button
3. This opens the topic creation form

---

## Topic Creation Form: Understanding Each Option

### Required Field: Topic Name

**Field:** Topic name box

**What to Enter:** A descriptive name for your topic

**Naming Conventions:**
- Use lowercase letters, numbers, hyphens, and underscores
- Maximum 256 characters
- Descriptive and meaningful names
- Examples:
  - `order-notifications`
  - `system-alerts`
  - `user-email-events`
  - `prod-payment-processing`

**Best Practices for Naming:**
- Include environment prefix: `prod-`, `staging-`, `dev-`
- Describe the purpose: `orders`, `alerts`, `notifications`
- Use hyphens for readability: `order-confirmation-emails`
- Avoid generic names: ❌ `topic1`, ✅ `order-topic`
- Keep it concise but descriptive: ❌ `everything-related-to-notifications`, ✅ `email-notifications`

**Examples:**
```
✓ prod-order-events
✓ staging-system-alerts
✓ dev-test-notifications
✓ prod-payment-status-updates
✓ marketing-campaign-events
```

### Required Field: Topic Type

**Options:** Standard or FIFO

**Standard Topic (Recommended for Most Use Cases)**
- Default topic type
- Best-effort ordering (generally in order, but not guaranteed)
- At-least-once delivery (may receive duplicates)
- Maximum throughput
- Lower cost
- Use when: Order of messages doesn't matter, or your application handles duplicates
- Examples: push notifications, marketing emails, system alerts

**FIFO Topic (First-In-First-Out)**
- Guarantees exact message order (FIFO delivery)
- Exactly-once processing (no duplicates)
- Lower throughput than Standard
- Higher cost than Standard
- Requires message group ID
- Use when: Message order is critical, no duplicates allowed
- Examples: financial transactions, inventory management, order processing where sequence matters

**For This Demo: Choose Standard**

**How to Choose:**
1. Click on the topic type dropdown/radio buttons
2. For this lesson, select "Standard"
3. If you need FIFO later, you can create a new FIFO topic

### Optional Field: Display Name

**Field:** Display name box

**What It Does:**
- Appears with SMS (text message) subscriptions
- Users see this name when they receive SMS messages
- Typically 10 characters or less (SMS limitations)
- Helps identify the message source

**When to Use:**
- If you plan to send SMS notifications: **Add a display name**
- If you only use email/HTTP/Lambda: **Can leave blank**

**Examples:**
- Topic: `order-notifications` → Display name: `MyStore`
- Topic: `system-alerts` → Display name: `AWS Ops`
- Topic: `marketing-emails` → Display name: `MyBrand`

**Best Practices:**
- Keep it short and recognizable
- Use your brand name or service name
- Avoid special characters that SMS might not support

### Configuration Settings (Optional)

**Additional settings available after clicking "Next step":**

1. **Encryption**
   - Server-Side Encryption (SSE)
   - AWS managed keys (default)
   - Customer-managed keys (if you need more control)
   - Can be configured now or later

2. **Access Policy**
   - Controls who can access the topic
   - Default: Only topic owner has access
   - Can restrict to specific IAM users/roles
   - Can configure later

3. **Delivery Status Logging**
   - Logs delivery attempts to CloudWatch
   - Useful for troubleshooting
   - Optional and can be configured later

4. **Tags**
   - Add metadata for organization
   - Useful for cost allocation
   - Example: `Environment: prod`, `Team: platform`

**For This Lesson: Leave Defaults**

All these settings can be configured later, so for the demo, we leave them as default.

---

## Step-by-Step: Create Topic Form Navigation

### Screen 1: Basic Information

```
╔═══════════════════════════════════════════╗
║         Create Topic - Step 1             ║
╠═══════════════════════════════════════════╣
║                                           ║
║ Topic Name:                               ║
║ [________________________________________] ║
║ (Enter your topic name)                  ║
║                                           ║
║ Topic Type:                               ║
║ ○ Standard                                ║
║ ○ FIFO                                    ║
║                                           ║
║ Display Name (optional):                  ║
║ [________________________________________] ║
║ (SMS display name)                        ║
║                                           ║
║ [ Cancel ]           [ Next step ]        ║
╚═══════════════════════════════════════════╝
```

**Actions:**
1. Enter topic name in first field
2. Select "Standard" for topic type
3. Enter display name (optional)
4. Click "Next step" button

### Screen 2: Configuration (Optional)

This screen shows optional configuration settings:
- Encryption settings
- Access policy
- Delivery status logging
- Tags

**For the demo: Skip these and go straight to create**

---

## Creating the Topic: The Final Step

### Click "Create Topic"

1. After reviewing configuration, click the **"Create topic"** button
2. AWS processes the request (usually takes 1-2 seconds)
3. A success banner appears confirming topic creation

### Success Confirmation

**What You'll See:**
```
✓ Topic created successfully

Topic ARN: arn:aws:sns:region:account-id:topic-name
Topic Name: [Your topic name]
```

**Important Information Shown:**
- **Topic ARN** – Amazon Resource Name (unique identifier)
- **Topic Name** – Name you entered
- **Region** – AWS region where topic was created
- **Account ID** – Your AWS account ID

**Save This Information:**
- The ARN will be needed when publishing messages or creating subscriptions
- You can always find it later in the Topic Details page

---

## After Topic Creation: What's Available

### Topic Details Page

Once created, you're taken to the topic details page. Here you can:

**Main Tabs/Sections:**
1. **Details** – Topic information, ARN, creation date, etc.
2. **Subscriptions** – Manage who receives messages from this topic
3. **Publish message** – Send test messages to the topic
4. **Access policy** – Control who can access this topic
5. **Encryption** – Configure message encryption
6. **Delivery status logging** – Enable CloudWatch logging
7. **Edit** – Modify topic settings

### Next Actions You Can Take

**1. Test the Topic (Recommended)**
- Click "Publish message"
- Enter a test message
- Click "Publish"
- Verify message was published (you'll see confirmation)

**2. Subscribe to the Topic**
- Click "Create subscription"
- Choose protocol (Email, SMS, HTTP, Lambda, etc.)
- Enter endpoint details
- Confirm subscription

**3. Configure Additional Settings**
- Add encryption if needed
- Set up access policies
- Enable logging

---

## Common Topic Creation Scenarios

### Scenario 1: E-Commerce Order Notifications

```
Topic Name: prod-order-notifications
Type: Standard
Display Name: MyShop
Purpose: Notify customers of order status updates
Subscribers: Email (customers), Lambda (order processor), SQS (warehouse)
```

### Scenario 2: System Monitoring Alerts

```
Topic Name: staging-system-alerts
Type: Standard
Display Name: AWS Alerts
Purpose: Alert operations team of infrastructure issues
Subscribers: Email (team), SMS (on-call), Lambda (auto-remediation)
```

### Scenario 3: Financial Transaction Processing

```
Topic Name: prod-payment-transactions
Type: FIFO (because order matters!)
Display Name: Payments
Purpose: Process payments in exact order received
Subscribers: Lambda (payment processor), SQS (audit), DynamoDB (history)
```

### Scenario 4: Mobile App Push Notifications

```
Topic Name: mobile-app-events
Type: Standard
Display Name: MyApp
Purpose: Send push notifications to mobile devices
Subscribers: Mobile Platform Endpoint (iOS/Android)
```

---

## Best Practices for Topic Creation

### 1. Naming Strategy
- ✅ Use environment prefix: `prod-`, `staging-`, `dev-`
- ✅ Be descriptive: `order-notifications`, not `topic-1`
- ✅ Use consistent format across organization
- ❌ Don't use: random names, vague names, special characters

### 2. Topic Type Selection
- ✅ Use **Standard** for: alerts, notifications, marketing, general messaging
- ✅ Use **FIFO** for: financial transactions, order processing, inventory
- ✅ Start with Standard, switch to FIFO only if needed
- ❌ Don't overuse FIFO (lower throughput, higher cost)

### 3. Display Names
- ✅ Add display name if using SMS
- ✅ Keep it short (10-20 chars)
- ✅ Use brand or service name
- ❌ Don't leave blank if sending SMS

### 4. Organization
- ✅ Use tags for cost allocation
- ✅ Create topics for specific purposes
- ✅ Group related topics by prefix
- ❌ Don't create one topic for everything

### 5. Security
- ✅ Use restrictive access policies
- ✅ Enable encryption for sensitive data
- ✅ Review access regularly
- ❌ Don't leave topics open to everyone

---

## Troubleshooting: Common Issues

### Issue: Topic Name Already Exists

**Problem:** "Topic with name [name] already exists"

**Causes:**
- Topic name is already used in this region
- Topic was created earlier and you forgot

**Solutions:**
- Choose a different topic name
- Add environment prefix if missing
- Check "Topics" list to see existing topics
- Topics are region-specific; same name OK in different regions

### Issue: Invalid Topic Name

**Problem:** "Topic name contains invalid characters"

**Causes:**
- Using invalid characters: `!@#$%^&*()`
- Using spaces without escaping
- Name too long (>256 chars)

**Solutions:**
- Use only: letters, numbers, hyphens (-), underscores (_)
- Avoid spaces; use hyphens instead
- Keep name under 256 characters
- Example: `my-order-events` ✓ vs `my order events` ✗

### Issue: Permission Denied

**Problem:** "User is not authorized to perform sns:CreateTopic"

**Causes:**
- IAM user doesn't have SNS permissions
- Using wrong AWS account
- Incorrect IAM policy

**Solutions:**
- Use AWS account owner or admin credentials
- Check IAM permissions for your user
- Ask account admin to add SNS permissions
- Policy needed: `sns:CreateTopic` action

### Issue: Can't Find SNS in Console

**Problem:** "Can't find SNS service in console"

**Solutions:**
- Use search bar: type "SNS"
- Check correct region (top-right selector)
- If using root account: make sure logged in
- Refresh the browser

---

## After Creating Your Topic

### Verify Topic Was Created

```
In the SNS Console:
1. Click "Topics" in left sidebar
2. Your new topic should appear in the list
3. Click on it to view details
4. Confirm the ARN and settings
```

### Next Steps (What Comes Next in Lesson 4)

1. **Subscribe to the topic** – Add email/SMS/Lambda subscribers
2. **Publish a message** – Send test message to verify
3. **Monitor delivery** – Check CloudWatch logs
4. **Configure access** – Set up IAM policies for access control

---

## Demo Walkthrough Summary

### The Workflow:

```
1. Open AWS Console
   ↓
2. Search for SNS
   ↓
3. Click "Simple Notification Service"
   ↓
4. Click "Create topic"
   ↓
5. Enter topic name
   ↓
6. Select topic type (Standard)
   ↓
7. Add display name (optional)
   ↓
8. Click "Next step"
   ↓
9. Review settings
   ↓
10. Click "Create topic"
    ↓
11. ✓ Success! Topic created
    ↓
12. Topic Details page opens
```

### Key Information to Save

After creation, note down:
- **Topic ARN:** `arn:aws:sns:region:account:topic-name`
- **Topic Name:** Your topic name
- **Region:** Region where created
- **Topic Type:** Standard or FIFO

---

## Key Takeaways

1. **Topics are the foundation** of SNS – everything starts with creating a topic

2. **Standard vs FIFO:**
   - Standard: Best for most use cases (notifications, alerts)
   - FIFO: Only when order matters and duplicates problematic

3. **Topic names should be:**
   - Descriptive and meaningful
   - Organized with prefixes (prod-, staging-, dev-)
   - Consistent across the organization

4. **Display names** are optional but recommended for SMS subscriptions

5. **Default settings are fine** for most use cases – configure advanced options later

6. **The ARN is important** – save it for use in subscriptions and policies

---

## Quiz Questions (Based on Demo)

### Question 1: Topic Type Selection
**Q:** When creating a topic for order notifications, which topic type should you choose?
- A) FIFO (because every order matters)
- B) Standard
- C) Premium
- D) Enterprise

**A:** B) Standard – Standard is best for notifications. FIFO is only needed if exact message order is critical.

### Question 2: Topic Naming
**Q:** What is a good practice for topic naming?
- A) Use descriptive names with environment prefix
- B) Use generic names like "topic-1"
- C) Use special characters for visibility
- D) Keep names as long as possible

**A:** A) Use descriptive names with environment prefix – This helps organize and identify topics across environments.

### Question 3: Display Name
**Q:** What is the display name field used for?
- A) Internal AWS tracking
- B) Appears in SMS messages to recipients
- C) Required for all topics
- D) Only for FIFO topics

**A:** B) Appears in SMS messages to recipients – The display name shows who the message is from in SMS subscriptions.

---

## Practice Exercise

### Create Your First Topic

**Exercise:** Create a topic for your use case

**Steps:**
1. Open AWS Management Console
2. Navigate to SNS
3. Click "Create topic"
4. Enter topic name: `my-first-topic` (or your choice)
5. Select topic type: `Standard`
6. Add display name: `My App`
7. Click "Next step"
8. Review settings
9. Click "Create topic"
10. Verify success message appears

**After Creation:**
- Note the Topic ARN
- Explore the Details page
- Try clicking "Publish message" to send a test message

**Success:** If you see the success banner and can view topic details, you've successfully created your first SNS topic! 🎉

---

## Next Lesson: Subscribing to Topics

In the next lesson, you'll learn:
- How to subscribe to SNS topics
- Different subscription protocols (email, SMS, Lambda, etc.)
- How to confirm subscriptions
- Testing subscriptions

**Prerequisite:** Having created a topic (which you just did!)

---

**Course Status:** Lessons 1-3 Complete ✅  
**Next:** Lesson 4 - Subscribing to SNS Topics  
**Last Updated:** September 11, 2026

---

## Quick Reference: Topic Creation Checklist

```
☐ Accessed AWS Management Console
☐ Navigated to SNS service
☐ Clicked "Create topic"
☐ Entered descriptive topic name
☐ Selected topic type (Standard or FIFO)
☐ Added display name (if using SMS)
☐ Reviewed configuration settings
☐ Clicked "Create topic"
☐ Saw success confirmation
☐ Noted the Topic ARN
☐ Explored Topic Details page
```

**Congratulations on creating your first SNS topic!** 🎊
