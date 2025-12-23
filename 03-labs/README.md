# `/03-labs/README.md`

## Purpose of This Lab Section

This section contains **hands-on labs** that validate how **AWS MCP actually behaves** when used with Amazon Q and real AWS accounts.

These labs are:

* executed against live AWS services
* grounded in documented MCP behavior
* focused on **control-plane operations**
* designed to surface **failures and recovery paths**

This is **not** a “happy-path-only” tutorial.

---

## What These Labs Are (And Are Not)

### These labs **ARE**:

* Real infrastructure deployments
* Executed via CloudFormation MCP server
* Backed by AWS IAM permissions
* Fully auditable in AWS consoles
* Repeatable with a clean environment

### These labs are **NOT**:

* Mock demos
* Simulated outputs
* Direct AWS SDK scripting
* Runtime application operations
* Kubernetes workload labs

---

## Execution Model Used in All Labs

Each lab follows the same execution flow:

```
Natural language intent
→ Amazon Q reasoning
→ MCP server tool invocation
→ AWS control-plane execution
→ Observation, failure handling, cleanup
```

This reflects the **actual MCP operating model**, not an abstraction.

---

## Safety and Cost Awareness

All labs:

* create real AWS resources
* require explicit approval before execution
* include cleanup steps
* emphasize cost control and resource deletion

You are expected to:

* monitor CloudFormation stacks
* verify resources in the AWS Console
* delete all test infrastructure after completion

---

## Lab Design Philosophy

Each lab intentionally demonstrates at least one of the following:

* declarative infrastructure creation
* dependency handling
* permission enforcement
* failure detection
* iterative repair using MCP
* clean teardown

This mirrors **real-world cloud operations**, not idealized demos.

---

## Prerequisites

Before running any lab:

* `/01-setup` must be completed
* AWS identity must be verified using `aws sts get-caller-identity`
* Amazon Q CLI must be logged in
* CloudFormation MCP server must be installed and configured

If these are not met, lab results are **undefined**.

---

## Why These Labs Exist

The goal is not to showcase Amazon Q.

The goal is to:

* understand where MCP adds value
* understand where it does not
* learn how GenAI fits into governed cloud operations
* build confidence in adopting MCP safely

---

## Note

If a lab fails:

* that is expected
* that is useful
* that is part of the learning

MCP is about **controlled iteration**, not blind automation.

---

