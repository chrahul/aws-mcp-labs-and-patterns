
# Lab 03 – Controlled Failure & Repair Loop (Circular Dependency)

## Objective

This lab demonstrates **how AWS MCP behaves when infrastructure provisioning fails**, and how **Amazon Q + CloudFormation MCP** handle:

* deployment failure
* error inspection
* root-cause analysis
* template correction
* safe redeployment

This lab is **not about breaking things randomly**.
It is about **proving that failure is a first-class, manageable state** in MCP-driven operations.

---

## Why This Lab Exists (Context for Leaders)

In real cloud environments:

* templates fail
* dependencies are misordered
* permissions are incomplete
* circular references happen

AWS MCP is designed **with this reality in mind**.

This lab validates that:

* MCP does not hide failures
* MCP does not “patch live resources”
* MCP fixes issues **declaratively and audibly**

---

## What This Lab Proves

By completing this lab, you will validate that:

* CloudFormation failures are fully visible to Amazon Q
* Stack events are used as the source of truth
* Amazon Q reasons over *actual AWS errors*
* Repairs happen by **modifying the template**
* Redeployment follows CloudFormation lifecycle rules
* Human approval is required at every execution step

---

## Prerequisites (Strict)

Before starting:

```bash
aws sts get-caller-identity
```

Must return:

* expected AWS account
* expected IAM user
* no SSO errors

Also ensure:

* Amazon Q CLI is logged in
* CloudFormation MCP server is installed and loaded
* Lab 01 and Lab 02 completed successfully

---

## Lab Context

We will intentionally create a **CloudFormation template with a circular dependency**, deploy it, observe failure, and then allow Amazon Q to **diagnose and repair** the template.

Example circular dependency patterns include:

* Lambda depends on IAM role
* IAM policy references Lambda ARN
* Resource permissions depend on each other

CloudFormation will reject this — correctly.

---

## Step 1: Start Amazon Q Chat

```bash
q chat
```

Ensure MCP servers are loaded during startup.

---

## Step 2: Intentionally Create a Faulty Template

Use a prompt similar to:

```
Create a CloudFormation template that includes:
- A Lambda function
- An IAM role for the Lambda
- Permissions that introduce a circular dependency between the role and the Lambda
Do not deploy yet.
```

### Expected Behavior

Amazon Q will:

* generate a YAML or JSON CloudFormation template
* explain the resource relationships
* not yet execute anything

At this point:

* no AWS resources exist
* this is still a planning phase

---

## Step 3: Deploy the Faulty Template

Ask Amazon Q:

```
Deploy this template as a CloudFormation stack named mcp-lab-03-failure-loop.
```

Approve execution when prompted.

---

## Step 4: Observe Stack Failure

Expected CloudFormation behavior:

* Stack enters `CREATE_IN_PROGRESS`
* Then transitions to `ROLLBACK_IN_PROGRESS`
* Eventually reaches `ROLLBACK_COMPLETE`

Amazon Q will surface:

* validation errors
* circular dependency messages
* failed resource names

This confirms:

* MCP does not suppress AWS errors
* CloudFormation remains authoritative

---

## Step 5: Analyze the Failure

Ask Amazon Q:

```
Analyze the CloudFormation stack failure and explain the root cause.
```

Amazon Q will:

* read CloudFormation stack events
* identify circular dependency
* explain why CloudFormation cannot resolve it

This step proves:

* MCP can *read and reason over real AWS failures*
* No assumptions or hallucinations are involved

---

## Step 6: Repair the Template (Key Learning)

Ask:

```
Fix the CloudFormation template to remove the circular dependency and redeploy the stack.
```

Amazon Q will:

* modify the template
* break the dependency loop
* possibly introduce explicit permissions or ordering
* wait for rollback completion if required
* ask for approval again

Approve the execution.

---

## Step 7: Verify Successful Deployment

Once deployment completes:

* Stack reaches `CREATE_COMPLETE`
* Resources are created successfully

From AWS Console:

* CloudFormation → Stack → Events
* Verify clean execution order
* Review Resources tab

This confirms:

* Repair happened at the **template level**
* No partial or unsafe changes were applied

---

## Step 8: Cleanup (Mandatory)

Delete the stack after verification.

### Option A – Via Amazon Q

```
Delete the CloudFormation stack mcp-lab-03-failure-loop.
```

Approve execution.

### Option B – Via AWS Console

* CloudFormation → Select stack → Delete

Ensure stack deletion completes successfully.

---

## Key Observations (Critical Learning)

* Failure is not hidden or bypassed
* MCP uses CloudFormation events as ground truth
* Repair is declarative, not imperative
* AI operates within strict execution boundaries
* Human approval remains mandatory

This behavior is **explicitly aligned** with AWS MCP design.

---

## What This Lab Intentionally Does NOT Do

* Does not auto-fix live resources
* Does not bypass CloudFormation
* Does not retry blindly
* Does not “force deploy” broken infrastructure

These omissions are **by design**.

---

## Why This Lab Matters

For enterprises, this lab answers the hardest question:

> *“What happens when AI-driven infrastructure goes wrong?”*

The answer demonstrated here:

* predictable failure
* transparent diagnosis
* controlled repair
* no loss of governance

This is the **minimum safety bar** for real adoption.

---

## Lab Outcome Summary

| Aspect                      | Result     |
| --------------------------- | ---------- |
| Intentional failure created | Yes        |
| Failure visibility          | Full       |
| Root cause analysis         | Yes        |
| Template-level repair       | Yes        |
| Redeployment                | Successful |
| Human approval enforced     | Yes        |
| Cleanup required            | Yes        |

---

## Next Step

**Lab 04 – Cleanup Discipline & Cost Safety**


