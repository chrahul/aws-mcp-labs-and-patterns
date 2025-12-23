# Lab 04 – Cleanup Discipline & Cost Safety (MCP Governance)

## Objective

This lab demonstrates how **cleanup and cost control remain explicit human responsibilities** when using **Amazon Q + AWS MCP**.

The goal is to validate that:

* MCP does not silently clean up resources
* AI does not make cost decisions on your behalf
* Infrastructure lifecycle discipline is mandatory
* Safe adoption requires deliberate teardown practices

This lab is about **operational maturity**, not tooling.

---

## Why This Lab Exists (Leadership Context)

AI can create infrastructure quickly.
The real risk is **what gets left behind**.

Most cloud cost overruns are not caused by:

* bad architecture
* expensive services

They are caused by:

* forgotten test stacks
* orphaned resources
* incomplete deletions

AWS MCP deliberately avoids automatic cleanup to prevent **unintended destructive actions**.

---

## What This Lab Proves

By completing this lab, you will validate that:

* Every resource created via MCP has a clear owner
* Cleanup is an explicit action, not a side effect
* CloudFormation remains the authoritative lifecycle manager
* Cost safety requires process, not AI heuristics

---

## Prerequisites

Before starting:

* Labs 01–03 completed
* One or more CloudFormation stacks exist from prior labs
* AWS identity verified (`aws sts get-caller-identity`)

---

## Lab Context

We will:

1. Identify all resources created in earlier labs
2. Attempt cleanup via Amazon Q
3. Handle common cleanup failures
4. Verify full deletion in AWS
5. Reflect on cost safety implications

---

## Step 1: Identify Active Resources

From AWS Console:

* Navigate to **CloudFormation**
* Review stacks created during Labs 01–03
* Note stack names and statuses

From Amazon Q, ask:

```
List all CloudFormation stacks created during previous MCP labs.
```

This reinforces that **CloudFormation is the system of record**.

---

## Step 2: Initiate Cleanup via Amazon Q

For each stack, ask:

```
Delete the CloudFormation stack <stack-name>.
```

Approve execution when prompted.

Amazon Q will:

* invoke CloudFormation delete
* monitor stack deletion
* report success or failure

---

## Step 3: Handle Cleanup Failures (Expected)

Common failure scenario:

* S3 bucket deletion fails due to remaining objects

Amazon Q will surface:

* CloudFormation error
* failed resource name
* reason for failure

This is expected and correct behavior.

---

## Step 4: Perform Manual Remediation

If S3 buckets block deletion:

* Navigate to **S3**
* Empty the bucket contents
* Retry stack deletion

This step demonstrates:

* AI does not perform destructive cleanup without instruction
* Humans remain accountable for irreversible actions

---

## Step 5: Verify Complete Deletion

Confirm:

* Stack status is `DELETE_COMPLETE`
* Stack no longer appears in CloudFormation
* Resources no longer appear in service consoles

This is the **only reliable definition of “cleanup complete.”**

---

## Step 6: Cost Safety Verification

Navigate to:

* AWS Cost Explorer (optional)
* CloudFormation → no active stacks
* S3 → no lab buckets

This ensures:

* No residual billable resources
* Clean account state

---

## Key Observations (Critical Learning)

* MCP does not auto-delete resources
* Cleanup requires explicit approval
* Failures are visible and actionable
* Cost safety is procedural, not automated

This behavior is **intentional** and aligns with AWS’s shared responsibility model.

---

## What This Lab Intentionally Does NOT Do

* Does not auto-expire resources
* Does not guess cost sensitivity
* Does not run background cleanup jobs
* Does not enforce budgets

These controls belong to **organizational governance**, not MCP.

---

## Why This Lab Matters

This lab answers a leadership-level concern:

> *“Can we trust AI-driven infrastructure changes in production?”*

The answer:

* Yes, **if** cleanup discipline exists
* No, **if** teams rely on automation without ownership

MCP supports **responsible acceleration**, not recklessness.

---

## Lab Outcome Summary

| Aspect                            | Result |
| --------------------------------- | ------ |
| Explicit cleanup required         | Yes    |
| CloudFormation lifecycle enforced | Yes    |
| Human approval mandatory          | Yes    |
| Cleanup failures visible          | Yes    |
| Cost safety maintained            | Yes    |

---

## Closing Reflection (For Leaders)

The value of MCP is not speed alone.

The real value is:

* controlled intent
* auditable execution
* predictable failure
* disciplined cleanup

This lab completes the **full infrastructure lifecycle story**.

---

## What’s Next

At this point, the repo has demonstrated:

* setup hygiene
* capability selection
* infra creation
* failure repair
* lifecycle cleanup

From here, natural next steps include:

* **EKS provisioning via CloudFormation MCP**
* **Incident triage with CloudWatch MCP**
* **Enterprise adoption models**

But those are *additions*, not prerequisites.

---

###  Note

If someone can complete all four labs responsibly,
they are ready to use MCP **in real environments**, not just demos.


