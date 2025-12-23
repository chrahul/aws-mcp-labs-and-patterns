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


/00-overview
  mcp-mental-model.md
  architecture-diagrams.md
  security-and-guardrails.md

/01-setup
  wsl-ubuntu-setup.md
  aws-cli-profiles.md
  amazon-q-cli-setup.md
  troubleshooting-auth-sso.md

/02-mcp-servers
  cfn-mcp-server.md
  cloudwatch-mcp-server.md
  server-selection-guide.md

/03-labs
  lab-01-s3-with-encryption-versioning.md
  lab-02-serverless-stack-apigw-lambda-s3.md
  lab-03-failure-repair-loop-circular-dependency.md
  lab-04-cleanup-and-cost-safety.md

/04-use-cases
  usecase-infra-from-intent.md
  usecase-incident-triage.md
  usecase-platform-guardrails.md

/05-leadership-notes
  executive-summary.md
  operating-model.md
  adoption-roadmap.md

