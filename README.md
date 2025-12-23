# aws-mcp-labs-and-patterns
aws-mcp-labs-and-patterns

##  Note

This repository is **not a tutorial for beginners**.

It is a **deep-dive exploration of agentic cloud operations** with:

* real failures
* real deployments
* real guardrails

The goal is not to “try MCP”,
but to **understand how GenAI fits into real cloud operating models**.

```
/00-overview
  mcp-mental-model
  architecture-diagrams
  security-and-guardrails

/01-setup
  wsl-ubuntu-setup
  aws-cli-profiles
  amazon-q-cli-setup
  troubleshooting-auth-sso

/02-mcp-servers
  cfn-mcp-server
  cloudwatch-mcp-server
  server-selection-guide

/03-labs
  lab-01-s3-with-encryption-versioning
  lab-02-serverless-stack-apigw-lambda-s3
  lab-03-failure-repair-loop-circular-dependency
  lab-04-cleanup-and-cost-safety

/04-use-cases
  usecase-infra-from-intent
  usecase-incident-triage
  usecase-platform-guardrails

/05-leadership-notes
  executive-summary
  operating-model
  adoption-roadmap

```

References:

* [https://docs.aws.amazon.com/aws-mcp/latest/userguide/getting-started-aws-mcp-server.html](https://docs.aws.amazon.com/aws-mcp/latest/userguide/getting-started-aws-mcp-server.html)
* [https://github.com/awslabs/mcp](https://github.com/awslabs/mcp)
* [https://awslabs.github.io/mcp/](https://awslabs.github.io/mcp/)
