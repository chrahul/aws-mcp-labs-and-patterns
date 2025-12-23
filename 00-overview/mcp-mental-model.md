

# `/00-overview`

## `mcp-mental-model.md`

### AWS MCP — The Correct Mental Model

AWS MCP (Model Context Protocol) is **not a new AWS service** and **not an AI automation shortcut**.
It is a **governed execution framework** that allows AI systems to interact with AWS **through controlled, auditable tools**.

Think of MCP as the **contract between intelligence and execution**.

### The Three Layers (Critical Understanding)

<img width="2848" height="1600" alt="image" src="https://github.com/user-attachments/assets/f8fa3918-ea9e-4587-84e1-39b70829a2fe" />


* **Amazon Q** reasons about *what should be done*
* **MCP servers** define *what actions are allowed*
* **AWS services** perform *the actual execution*

MCP does **not**:

* bypass IAM
* bypass CloudFormation
* bypass AWS APIs

It **wraps them safely**.

---

### MCP Servers Are Capability-Based (Not Service-Based)

A common misconception is that MCP servers map 1:1 with AWS services.

That is **incorrect**.

MCP servers are grouped by **operational capability**, not by service name.

| Capability                  | MCP Server         |
| --------------------------- | ------------------ |
| Infrastructure provisioning | CloudFormation MCP |
| Monitoring & diagnostics    | CloudWatch MCP     |
| Cost & usage analysis       | Cost Explorer MCP  |
| Security posture            | Security Hub MCP   |

There is **no EKS MCP server** today — by design.

Why?
Because EKS creation and management is an **orchestration problem**, not a single-service action.

---

### Why CloudFormation Is Central to MCP

For infrastructure operations, AWS deliberately chose **CloudFormation** as the execution plane because it provides:

* Declarative intent
* Dependency resolution
* Rollback on failure
* Audit trail
* Idempotent execution

MCP does not directly call low-level APIs like `CreateBucket` or `CreateCluster`.
Instead, it **generates and applies CloudFormation templates**.

This is what makes MCP **enterprise-safe**.

---

### What MCP Is NOT

Understanding limitations is as important as understanding capabilities.

MCP is not:

* A replacement for `kubectl`
* A replacement for GitOps tools
* A replacement for CI/CD
* A free-form AI automation engine

MCP focuses on **control-plane workflows**, not runtime operations.

---

### When MCP Is the Right Tool

Use MCP when:

* You want to provision or modify infrastructure
* You need auditability and rollback
* You want AI-assisted repair and iteration
* You need guardrails around AI actions

Do not use MCP when:

* Managing live Kubernetes workloads
* Debugging application-level issues
* Running ad-hoc scripts without governance

---

## `architecture-diagrams.md`

### High-Level Architecture

<img width="2848" height="1600" alt="image" src="https://github.com/user-attachments/assets/530f18a8-73aa-4659-b7e4-09924b80c472" />


Key points:

* MCP servers run **locally or in controlled environments**
* AWS credentials are resolved via **AWS CLI profiles**
* Amazon Q never receives raw credentials
* Every action is permission-checked

---

### Control Plane vs Runtime Plane

| Plane         | Tooling                      |
| ------------- | ---------------------------- |
| Control Plane | MCP + CloudFormation         |
| Runtime Plane | kubectl, GitOps, app tooling |

This separation is intentional and critical for safety.

---

### Failure Is a First-Class Citizen

Unlike scripts, MCP workflows:

* expect failures
* inspect stack events
* modify intent
* reapply changes

This enables **self-healing infrastructure workflows** without losing control.

---

## `security-and-guardrails.md`

### Security Principles in MCP

MCP is designed around **explicit trust and scoped authority**.

Key principles:

* No implicit execution
* No hidden permissions
* No background autonomy

Every action requires:

* Tool approval
* IAM permission
* Explicit execution

---

### IAM Is the Ultimate Boundary

MCP does not introduce a new security model.

All execution is bound by:

* AWS CLI profile
* IAM policies
* AWS service permissions

If IAM denies an action, MCP cannot override it.

---

### Environment Hygiene Matters

Because MCP relies on the local AWS CLI context:

* Environment variables matter
* Profiles matter
* SSO remnants can cause confusion

This repo intentionally includes:

* profile isolation
* SSO cleanup guidance
* deterministic setups

---

### Why This Matters for Enterprises

Enterprises don’t fear AI — they fear **uncontrolled automation**.

MCP solves this by:

* forcing declarative intent
* centralizing execution
* making AI actions inspectable

This is the same evolution path:

* scripts → CI/CD
* manual ops → IaC
* free AI → governed AI

---


---

### Next Step

From here, the repo naturally flows into:

`/01-setup` — building a **clean, reproducible execution environment**.


