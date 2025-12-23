
# Lab 01 – Create S3 Bucket with Encryption & Versioning (via CloudFormation MCP)

## Objective

This lab demonstrates how **Amazon Q**, when paired with the **AWS CloudFormation MCP server**, can provision real AWS infrastructure **using natural language**, while still enforcing:

* declarative infrastructure
* IAM permissions
* auditability
* explicit user approval

The goal is **not** to learn S3, but to understand **how MCP executes infrastructure intent**.

---

## What This Lab Proves

By the end of this lab, you will have verified that:

* Amazon Q does **not** call S3 APIs directly
* Infrastructure creation is executed **only via CloudFormation**
* MCP requires explicit approval before execution
* The resulting resources are fully visible and auditable in AWS
* Cleanup is manual and intentional

---

## Prerequisites (Strict)

Before starting, confirm all of the following:

```bash
aws sts get-caller-identity
```

Expected:

* Correct AWS account
* Correct IAM user
* No SSO errors

Amazon Q CLI must be:

* installed
* logged in
* able to run AWS commands successfully

CloudFormation MCP server must be:

* installed
* configured
* loaded by Amazon Q

If any of these are not true, **do not proceed**.

---

## Lab Context

We will create an **Amazon S3 bucket** with:

* server-side encryption enabled
* versioning enabled

Important:

* S3 bucket names are **globally unique**
* Use a unique suffix to avoid name collisions

---

## Step 1: Start Amazon Q Chat

```bash
q chat
```

Ensure MCP servers are loaded during startup.

---

## Step 2: Provide Intent (Natural Language)

Use a prompt similar to the following:

```
Create an S3 bucket with server-side encryption and versioning enabled.
Use a globally unique bucket name.
```

You may optionally specify a name:

```
Create an S3 bucket named rahul-mcp-lab-01-<unique-suffix> with encryption and versioning enabled.
```

---

## Step 3: Observe Q’s Reasoning (Important)

Amazon Q will:

1. Interpret the intent
2. Choose the **CloudFormation MCP server**
3. Generate a **CloudFormation template**
4. Present the planned action
5. Ask for **explicit permission** to execute

At this stage:

* No AWS resources have been created
* No APIs have been called

This approval gate is a **core MCP safety mechanism**.

---

## Step 4: Approve Execution

When prompted, approve the action.

Amazon Q will then:

* invoke the CloudFormation MCP tool
* create a CloudFormation stack
* wait for stack completion

---

## Step 5: Monitor Execution

You may observe progress in two ways:

### Option A: From Amazon Q output

* Stack creation status
* Success or failure messages

### Option B: From AWS Console

* Navigate to **CloudFormation**
* Locate the newly created stack
* Review **Events** and **Resources**

This confirms:

* MCP execution is fully visible
* No hidden or background actions exist

---

## Step 6: Verify the S3 Bucket

In the AWS Console:

* Go to **S3**
* Locate the bucket
* Confirm:

  * Versioning is enabled
  * Default encryption is enabled

These settings should match the declared intent.

---

## Key Observations (Critical Learning)

* Amazon Q did **not** create the bucket directly
* A CloudFormation stack was the execution unit
* The bucket lifecycle is now tied to that stack
* Infrastructure is **declarative**, not procedural

This is the **intended MCP behavior**, as documented.

---

## Step 7: Cleanup (Mandatory)

Delete the infrastructure to avoid unnecessary cost.

### Option A: Via Amazon Q

Ask:

```
Delete the CloudFormation stack created for this lab.
```

Approve execution.

### Option B: Via AWS Console

* Go to **CloudFormation**
* Select the stack
* Choose **Delete**

If deletion fails due to:

* non-empty bucket

Then:

* Empty the S3 bucket manually
* Retry stack deletion

---

## What This Lab Intentionally Did NOT Do

* Did not use AWS SDKs
* Did not use `aws s3` CLI commands
* Did not bypass CloudFormation
* Did not automate cleanup silently

These omissions are **by design**.

---

## Why This Lab Matters

This simple example establishes a foundation:

* MCP is about **governed infrastructure changes**
* AI assists with intent and iteration
* CloudFormation enforces correctness and safety
* Humans remain in control

Every subsequent lab builds on this model.

---

## Lab Outcome Summary

| Aspect                 | Result             |
| ---------------------- | ------------------ |
| Infrastructure created | Yes                |
| Execution method       | CloudFormation MCP |
| User approval required | Yes                |
| Audit trail available  | Yes                |
| Cleanup required       | Yes                |

---

## Next Lab

 **Lab 02 – Serverless Stack (API Gateway + Lambda + S3)**
Where we move from single-resource provisioning to **multi-resource orchestration and dependency handling**.

---

