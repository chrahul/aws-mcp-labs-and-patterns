

# Lab 02 – Serverless Stack (API Gateway + Lambda + S3) via CloudFormation MCP

## Objective

This lab demonstrates **multi-resource orchestration** using **Amazon Q + CloudFormation MCP** to deploy a **serverless stack** consisting of:

* Amazon API Gateway (REST)
* AWS Lambda
* Amazon S3
* Required IAM roles and permissions

The focus is on **dependency handling, failure visibility, iterative repair, and governed execution**—not on application logic.

---

## What This Lab Proves

By completing this lab, you will validate that:

* MCP uses **CloudFormation as the orchestration engine**
* Amazon Q can **generate non-trivial templates**
* Cross-service dependencies are resolved declaratively
* Failures surface via **CloudFormation events**
* Iterative fixes occur through **template modification**, not ad-hoc API calls
* Cleanup remains an explicit human decision

---

## Prerequisites (Strict)

Before starting, confirm:

```bash
aws sts get-caller-identity
```

* Correct AWS account
* Correct IAM user
* No SSO errors

Also ensure:

* Amazon Q CLI is logged in
* CloudFormation MCP server is installed and loaded
* Lab 01 completed successfully

If any prerequisite fails, **do not proceed**.

---

## Lab Context

We will ask Amazon Q to:

1. Generate a **YAML CloudFormation template**
2. Define:

   * REST API (API Gateway)
   * Lambda function
   * S3 bucket
   * IAM execution role
3. Deploy the stack
4. Observe failures (if any)
5. Allow Q to repair and redeploy
6. Validate resources
7. Clean up

---

## Step 1: Start Amazon Q Chat

```bash
q chat
```

Confirm MCP servers load during startup.

---

## Step 2: Provide Intent (Template Creation)

Use the following prompt (or equivalent):

```
Create a YAML CloudFormation template for a serverless stack with:
- API Gateway REST API
- Lambda function
- S3 bucket
Include necessary IAM roles and permissions.
Do not deploy yet.
```

### Expected Behavior

Amazon Q will:

* Select the **CloudFormation MCP server**
* Generate a **YAML template**
* Explain included resources
* Stop short of execution

At this stage:

* No AWS resources are created
* This is a **planning step only**

---

## Step 3: Review the Generated Template

Review the template output carefully.

Key things to confirm:

* IAM role exists for Lambda execution
* API Gateway permission to invoke Lambda
* S3 bucket is defined declaratively
* Outputs are defined (optional but useful)

This reinforces that **intent becomes declarative specification**, not imperative scripts.

---

## Step 4: Deploy the Stack

Ask Amazon Q to deploy:

```
Deploy this template as a CloudFormation stack named mcp-lab-02-serverless.
```

Amazon Q will:

1. Ask for explicit permission
2. Invoke CloudFormation MCP tools
3. Create the stack
4. Monitor deployment

Approve execution when prompted.

---

## Step 5: Observe Deployment (Critical Learning)

During deployment, one of two things will happen:

### Case A – Stack Succeeds

* Stack reaches `CREATE_COMPLETE`
* Resources are created successfully

### Case B – Stack Fails (Very Common)

* Stack enters `ROLLBACK_IN_PROGRESS`
* Error messages appear (e.g., permissions, circular dependencies)

This is **expected and valuable**.

---

## Step 6: Inspect Failure (If Any)

If the stack fails, ask:

```
Analyze the CloudFormation stack failure and explain the root cause.
```

Amazon Q will:

* Read CloudFormation stack events
* Identify dependency or permission issues
* Explain what went wrong

This confirms:

* MCP can **read and reason over failures**
* No guessing or hidden logic is involved

---

## Step 7: Allow Iterative Repair

Ask:

```
Fix the CloudFormation template to resolve the issue and redeploy the stack.
```

Amazon Q will:

* Modify the template
* Wait for rollback completion (if required)
* Reattempt deployment
* Ask for approval again

This repair loop is **one of the most important MCP behaviors**.

---

## Step 8: Verify Successful Deployment

Once the stack reaches `CREATE_COMPLETE`:

### From AWS Console

* Open **CloudFormation**
* Select the stack
* Review:

  * Resources
  * Events
  * Outputs

You should see:

* API Gateway REST API
* Lambda function
* S3 bucket
* IAM role

Optionally:

* Invoke the API endpoint (if exposed)
* Confirm Lambda execution logs in CloudWatch

---

## Key Observations (Important)

* Multiple AWS services were orchestrated via **one declarative stack**
* Failures were handled at the **template level**
* Amazon Q did not “hotfix” resources directly
* CloudFormation remained the single source of truth

This behavior aligns exactly with:

* AWS MCP documentation
* awslabs CloudFormation MCP implementation

---

## Step 9: Cleanup (Mandatory)

Delete the stack to avoid ongoing cost.

### Option A: Via Amazon Q

```
Delete the CloudFormation stack mcp-lab-02-serverless.
```

Approve execution.

### Option B: Via AWS Console

* CloudFormation → Select stack → Delete

If deletion fails due to:

* Non-empty S3 bucket

Then:

* Empty the bucket manually
* Retry deletion

---

## What This Lab Intentionally Does NOT Cover

* API implementation details
* Lambda business logic
* Runtime debugging
* Performance tuning

Those belong outside MCP’s scope.

---

## Why This Lab Matters

This lab demonstrates that **GenAI + MCP is viable for real infrastructure orchestration**, not just toy examples.

It shows:

* dependency awareness
* failure transparency
* iterative correction
* governance over automation

This is the **minimum bar for enterprise adoption**.

---

## Lab Outcome Summary

| Aspect                       | Result             |
| ---------------------------- | ------------------ |
| Multi-resource orchestration | Yes                |
| Execution method             | CloudFormation MCP |
| Failure visibility           | Yes                |
| Iterative repair             | Yes                |
| Human approval enforced      | Yes                |
| Cleanup required             | Yes                |

---

## Next Lab

**Lab 03 – Failure & Repair Loop (Circular Dependency)**
Where failure is **intentional**, and recovery is the primary learning objective.


