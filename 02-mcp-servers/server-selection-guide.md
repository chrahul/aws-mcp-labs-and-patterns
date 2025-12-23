# `/02-mcp-servers`

## Purpose of This Section

This section answers **one core question**:

> *How do MCP servers actually work, what exists today, and how do you choose the right one — without guessing or overengineering?*

AWS MCP servers are **explicit, opt-in capability providers**.
Nothing happens automatically. Nothing is inferred.



<img width="2848" height="1600" alt="image" src="https://github.com/user-attachments/assets/71be8b7e-19d8-43f9-b6e5-d07ca9f6420f" />


---

## `server-selection-guide.md`

### The Most Important Rule (From AWS Docs)

> **Amazon Q does nothing unless an MCP server explicitly exposes a tool for it.**

This is stated implicitly across:

* the MCP overview
* the server documentation
* and observable behavior in real execution

There is:

*  no auto-discovery of AWS services
*  no “generic AWS MCP”
*  no per-service auto-mapping

You only get what a server **implements**.

---

### How AWS MCP Servers Are Organized 

From awslabs MCP:

* MCP servers are grouped by **operational domain**
* Each server exposes a **finite, documented toolset**
* Each tool maps to **specific AWS control-plane operations**

They are **not** created per AWS service.

---

### Current AWS MCP Servers 

| MCP Server         | Operational Scope                       |
| ------------------ | --------------------------------------- |
| CloudFormation MCP | Infrastructure provisioning & lifecycle |
| CloudWatch MCP     | Metrics, logs, alarms, observability    |
| Cost Explorer MCP  | Cost & usage analysis                   |
| Security Hub MCP   | Security posture & findings             |

> This list comes directly from awslabs MCP documentation and the AWS MCP User Guide.

No hidden servers. No implied servers.

---

### Why This Matters

If a tool is **not listed** under an MCP server:

* Amazon Q **cannot** execute it
* Even if AWS has an API for it
* Even if you have IAM permission

This is deliberate and documented behavior.

---

## `cfn-mcp-server.md`

### What the CloudFormation MCP Server Is (Documented)

From the CloudFormation MCP server documentation:

> The CloudFormation MCP server exposes tools that allow models to create, update, validate, and delete CloudFormation stacks.

Key capabilities:

* Template validation
* Stack creation
* Stack updates
* Stack deletion
* Stack event inspection
* Resource listing

All execution goes through **AWS CloudFormation**.

---

### What It Does NOT Do (Important)

CloudFormation MCP:

*  does not call raw service APIs (e.g. CreateBucket)
*  does not bypass CloudFormation
*  does not manage runtime services

It strictly operates at the **infrastructure control plane**.

---

### Why CloudFormation MCP Covers Many Services

Because CloudFormation templates can define:

* S3
* Lambda
* API Gateway
* IAM
* EC2
* EKS
* VPC
* CloudWatch resources

So CloudFormation MCP becomes the **single infra entry point**.

This is not an assumption — this is a direct result of how CloudFormation works.

---

### Real Behavior (Observed, Not Assumed)

In our labs:

* Amazon Q generated CloudFormation templates
* Asked permission before execution
* Deployed stacks
* Detected failures
* Inspected stack events
* Modified templates
* Re-deployed successfully

This behavior matches:

* AWS MCP User Guide
* awslabs cfn-mcp-server documentation

---

## `cloudwatch-mcp-server.md`

### What the CloudWatch MCP Server Is (Documented)

From awslabs MCP:

> The CloudWatch MCP server exposes tools for interacting with metrics, logs, and alarms.

This includes:

* Querying metrics
* Reading logs
* Inspecting alarms
* Observing operational state

It **does not**:

* deploy infrastructure
* modify resources
* execute remediation actions

---

### How It Fits in the MCP Model

CloudWatch MCP answers:

> *“What is happening?”*

CloudFormation MCP answers:

> *“What should be created or changed?”*

They are complementary, not overlapping.

---

## How to Choose the Right MCP Server (No Guessing)

Ask **one question**:

> *What kind of control-plane action do I want?*

| Intent                         | MCP Server         |
| ------------------------------ | ------------------ |
| Create / modify infrastructure | CloudFormation MCP |
| Inspect metrics / logs         | CloudWatch MCP     |
| Analyze spend                  | Cost Explorer MCP  |
| Review security findings       | Security Hub MCP   |

This decision table is **fully consistent with AWS docs**.

---

## What MCP Servers Are Explicitly NOT Meant For

Based on AWS documentation and absence of tooling:

MCP servers do **not** handle:

* Kubernetes runtime operations (`kubectl`)
* Application debugging
* CI/CD pipelines
* Ad-hoc scripting

AWS draws a **hard boundary** at the control plane.

---

## The Non-Existence of “EKS MCP Server” (Fact)

There is:

*  no EKS MCP server in awslabs
*  no mention of one in AWS MCP docs

Why this matters:

* EKS creation is an **infrastructure orchestration** task
* CloudFormation already covers it
* AWS chose **one orchestrator**, not many service-level servers

This is **observed reality**, not interpretation.

---

## 

> “AWS MCP servers are capability-based executors. For infrastructure provisioning, AWS provides the CloudFormation MCP server, which allows Amazon Q to generate, deploy, and repair CloudFormation stacks. There is no per-service MCP server model.”

---

## Why This Design Is Intentional 

From what exists (and what doesn’t), AWS is clearly optimizing for:

* minimal blast radius
* auditable actions
* deterministic execution
* enterprise safety

Not convenience automation.

---

## End of `/02-mcp-servers`

At this point, a reader should clearly understand:

* what MCP servers are
* what exists today
* how to select one
* why CloudFormation MCP is central
* what MCP explicitly does not do

---

### Next Section

 `/03-labs`
Where we move from **capability understanding** to **real execution, failures, and recovery**, using only documented behavior and real AWS results.




