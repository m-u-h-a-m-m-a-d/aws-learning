# SNS Topic Creation - Practical Hands-On Guide

**Lesson:** 3 - Creating an Amazon SNS Topic  
**Type:** Step-by-Step Implementation Guide  
**Duration:** 15-30 minutes

---

## 🎯 What You'll Create

By the end of this guide, you'll have created a working SNS topic in your AWS account and tested it.

**Final Result:**
- ✅ One working SNS topic
- ✅ Topic ARN saved
- ✅ Test message published
- ✅ Foundation for adding subscriptions

---

## Prerequisites

Before starting, you need:
- Active AWS account (with admin or SNS permissions)
- Access to AWS Management Console
- Basic familiarity with AWS interface

---

## Part 1: Accessing AWS SNS Console

### Step 1A: Log into AWS Console

1. Open your browser
2. Go to: https://console.aws.amazon.com
3. Sign in with your AWS credentials
4. You're now on the AWS Console home page

### Step 1B: Navigate to SNS

**Using Search (Recommended):**
1. Look at the top of the console for the search bar
2. Click on the search box
3. Type: `SNS`
4. Press Enter or click "Simple Notification Service" from results
5. SNS console opens

**Visual Cue:**
```
Top of console:
┌──────────────────────────────────────┐
│ 🔍 Search bar (type SNS here)         │
└──────────────────────────────────────┘
     ↓
Click "Simple Notification Service"
     ↓
SNS Console opens
```

---

## Part 2: Create Your First Topic

### Step 2A: Initiate Topic Creation

1. In the SNS console, look for the **"Create topic"** button
   - Usually in top-right area
   - Blue/purple colored button
   - Text: "Create topic"

2. Click the "Create topic" button

**What You Should See:**
```
SNS Console
├─ Left sidebar with Topics, Subscriptions, etc.
├─ Main area shows "Create topic" button
└─ Form appears when you click button
```

### Step 2B: Fill in Topic Name

**Field 1: Topic name**

```
Topic name:
[________________    ]
Enter name here
```

**What to Enter:**
Choose one based on your use case:

| Use Case | Suggested Name |
|----------|-----------------|
| Learning/Testing | `my-test-topic` |
| E-commerce | `prod-order-notifications` |
| Alerts | `staging-system-alerts` |
| Marketing | `dev-email-campaigns` |

**Example:** For this guide, enter:
```
my-first-sns-topic
```

**Rules:**
- Use lowercase letters, numbers, hyphens (-), underscores (_)
- Maximum 256 characters
- Must be unique in your region
- No spaces (use hyphens instead)

### Step 2C: Select Topic Type

**Field 2: Topic type**

You'll see two options:

```
○ Standard (recommended)
○ FIFO
```

**For This Exercise: Select "Standard"**

**Comparison:**
| Feature | Standard | FIFO |
|---------|----------|------|
| Order | Best-effort | Guaranteed |
| Duplicates | May occur | Never |
| Throughput | Higher | Lower |
| Cost | Lower | Higher |
| Use For | General messaging | Critical ordering |

**Click:** Select the radio button next to "Standard"

### Step 2D: Add Display Name (Optional)

**Field 3: Display name**

```
Display name:
[________________    ]
Optional - for SMS
```

**What to Enter:**
A short name that appears in SMS messages

**Example:** For this topic, enter:
```
MyApp
```

**Tips:**
- Keep it 10-20 characters
- Use your app or brand name
- Only matters if you send SMS
- Leave blank if no SMS subscriptions

---

## Part 3: Complete Topic Creation

### Step 3A: Review Settings

Before clicking create, your form should look like:

```
Topic name: my-first-sns-topic
Topic type: ○ Standard (selected)
Display name: MyApp
```

### Step 3B: Click Create Topic

1. Look for the **"Create topic"** button at the bottom of the form
   - Sometimes there's a "Next step" first
   - If "Next step" appears, click it to proceed
   - Then click "Create topic" on the next screen

2. Click the button

3. Wait for processing (usually 1-2 seconds)

### Step 3C: Confirm Success

**You should see:**
```
✓ Topic created successfully

Topic ARN: arn:aws:sns:us-east-1:123456789012:my-first-sns-topic
Topic Name: my-first-sns-topic
```

**Save This Information:**

Copy and save the Topic ARN somewhere (notepad, etc.):
```
ARN: arn:aws:sns:REGION:ACCOUNT-ID:my-first-sns-topic
```

You'll need this for subscriptions and future work.

---

## Part 4: Explore Your Topic

### Step 4A: View Topic Details

After creation, you're on the Topic Details page. You should see:

**Sections available:**
- **Details** – Topic info, ARN, creation date
- **Subscriptions** – (empty for now)
- **Publish message** – Test publishing
- **Access policy** – Permission settings
- **Encryption** – Message encryption settings
- **Delivery status logging** – CloudWatch logging

### Step 4B: Verify Topic Information

Look for these details on the page:

```
Topic Details
├─ Topic ARN: arn:aws:sns:...
├─ Topic Name: my-first-sns-topic
├─ Type: Standard
├─ Creation Date: [date and time]
├─ Display Name: MyApp
└─ Status: Active
```

**✓ If you see all this, your topic is ready!**

---

## Part 5: Test Your Topic

### Step 5A: Publish a Test Message

1. Scroll down to find the section called **"Publish message"**
2. Click on it or the **"Publish message"** button

### Step 5B: Enter Message Details

**Form appears with:**

**Message title (optional):**
```
[Enter subject or title here]
```
Example: `Test Message`

**Message body:**
```
[Enter message content here]
```
Example:
```
Hello! This is a test message for my SNS topic.
If you see this, the topic is working correctly!
```

**Message structure:**
```
Subject: Test Message
Body: Hello! This is a test message for my SNS topic.
```

### Step 5C: Publish the Message

1. Scroll to the bottom of the form
2. Click the blue **"Publish message"** button
3. Wait for confirmation

### Step 5D: Verify Message Published

**You should see:**
```
✓ Message published successfully

Message ID: abc123def456...
```

**Success!** Your topic is working. Messages can now be published to it.

---

## Part 6: Return to Topics List

### View All Your Topics

1. Click **"Topics"** in the left sidebar
2. Your new topic appears in the list
3. You can see:
   - Topic name
   - Topic ARN
   - Number of subscriptions (currently 0)
   - Creation date

**Your topic is now ready for subscriptions!**

---

## Troubleshooting Guide

### Problem 1: Topic Name Already Exists

**Error:** "Topic with name already exists"

**Solution:**
1. Topic name is taken in this region
2. Add a prefix: `prod-my-first-topic` or `test-my-first-topic`
3. Or use a different name
4. Try again with new name

### Problem 2: Permission Denied

**Error:** "User is not authorized to perform SNS:CreateTopic"

**Solution:**
1. You don't have SNS permissions
2. Use an account with admin access, or
3. Ask account administrator to add SNS permissions to your IAM user
4. Required permission: `sns:CreateTopic`

### Problem 3: Can't Find SNS Service

**Solution:**
1. Make sure you're in the right region (top-right selector)
2. Type "SNS" in search bar (case-insensitive)
3. Try refreshing the page (F5)
4. Check if you're logged in to correct AWS account

### Problem 4: Search Not Working

**Solution:**
1. Click directly in the search box first
2. Wait for it to activate
3. Then type "SNS"
4. If search dropdown doesn't appear, try refresh

---

## Verification Checklist

After completing all steps, verify:

```
✓ AWS Console is open and I'm logged in
✓ I navigated to SNS service
✓ I clicked "Create topic"
✓ I entered topic name: my-first-sns-topic
✓ I selected topic type: Standard
✓ I added display name: MyApp (optional)
✓ I clicked "Create topic"
✓ Success message appeared
✓ Topic ARN is visible
✓ I copied and saved the ARN
✓ I published a test message
✓ Test message confirmed success
✓ Topic appears in "Topics" list
```

**If all boxes checked: You're done! 🎉**

---

## What You've Accomplished

### Completed Tasks:
1. ✅ Accessed AWS SNS Console
2. ✅ Created your first SNS topic
3. ✅ Chose appropriate topic type
4. ✅ Added display name for SMS
5. ✅ Published a test message
6. ✅ Verified successful message delivery

### What's Next:
- **Next lesson:** Subscribe to topics (add email, SMS, or other subscribers)
- **Future:** Integrate with Lambda, SQS, and other AWS services
- **Advanced:** Configure access policies and encryption

---

## Important Information to Keep

**Save these details for future use:**

```
Topic Name: my-first-sns-topic
Region: us-east-1 (or your region)
Topic ARN: arn:aws:sns:REGION:ACCOUNT:my-first-sns-topic
Topic Type: Standard
Created: [date]
```

Use the ARN whenever you need to:
- Add subscriptions
- Set access policies
- Configure integrations
- Reference the topic in code

---

## Common Next Steps

### Option 1: Add Email Subscription
1. Go to your topic
2. Click "Create subscription"
3. Choose "Email"
4. Enter your email address
5. Confirm email (check inbox)

### Option 2: Add SMS Subscription
1. Go to your topic
2. Click "Create subscription"
3. Choose "SMS"
4. Enter phone number
5. Test with published message

### Option 3: Add Lambda Subscription
1. Go to your topic
2. Click "Create subscription"
3. Choose "AWS Lambda"
4. Select your Lambda function
5. Lambda runs when messages published

---

## Quick Reference: Console Navigation

**Getting back to your topic:**

```
AWS Console
├─ Search "SNS"
├─ Click "Simple Notification Service"
├─ Left sidebar: Click "Topics"
├─ Find your topic in the list
├─ Click on it to see details
└─ You can now publish or subscribe
```

**Or bookmark the direct link:**
- AWS Console → SNS → Topics (for quick access)

---

## Best Practices Recap

### For Future Topics:

1. **Naming:**
   - ✅ Use environment prefix: `prod-`, `staging-`, `dev-`
   - ✅ Be descriptive: `order-notifications`
   - ❌ Don't use: `topic1`, `temp`, `test2`

2. **Type Selection:**
   - ✅ Start with Standard
   - ✅ Only use FIFO if order matters
   - ❌ Don't overuse FIFO

3. **Organization:**
   - ✅ Keep names consistent across team
   - ✅ Document topic purposes
   - ✅ Use naming conventions

---

## Success Indicators

**Your topic is properly created if:**

| Check | ✓ Yes | ✗ No |
|-------|-------|------|
| Topic appears in list | ✓ | ✗ |
| Topic has valid ARN | ✓ | ✗ |
| Can publish messages | ✓ | ✗ |
| Success message appeared | ✓ | ✗ |
| Topic shows as "Active" | ✓ | ✗ |

**If all are ✓: Success!** Your topic is ready for subscriptions.

---

## Getting Help

If you get stuck:

1. **Error messages:** Read carefully – they tell you what's wrong
2. **Permissions:** Check IAM policies if access denied
3. **Region:** Make sure correct region is selected
4. **Account:** Verify you're in the right AWS account
5. **AWS Documentation:** Visit docs.aws.amazon.com/sns

---

**Congratulations on creating your first SNS topic!** 🎊

You've completed the hands-on portion of Lesson 3.

**Next:** Learn how to add subscriptions to receive messages from your topic.

---

**Course:** Amazon SNS, Getting Started  
**Lesson:** 3 - Creating an Amazon SNS Topic  
**Status:** ✅ Complete  
**Last Updated:** September 11, 2026
