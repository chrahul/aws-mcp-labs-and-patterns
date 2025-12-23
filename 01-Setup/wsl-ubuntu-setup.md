# `/01-setup`

## Purpose of This Section

This setup is **intentionally strict**.

MCP relies on:

* local execution
* AWS CLI context
* environment variables
* explicit trust

If the environment is polluted (old SSO, mixed profiles, Windows/WSL confusion), **AI-assisted operations become dangerous and misleading**.

This section ensures:

* one identity
* one profile
* one execution context
* zero ambiguity

---

## `wsl-ubuntu-setup.md`

### Why WSL + Ubuntu Is Mandatory

Amazon Q CLI and AWS MCP servers are **Linux-first tools**.

Running them from:

* Windows CMD
* PowerShell
* Git Bash

will lead to:

* broken installs
* missing `sudo`
* incompatible binaries
* silent failures

**Ubuntu on WSL2 is the supported baseline.**

---

### Install WSL (Windows)

From PowerShell (Admin):

```powershell
wsl --install
```

Restart if prompted.

Launch Ubuntu:

```powershell
wsl -d Ubuntu
```

---

### Verify Linux Environment

Inside Ubuntu:

```bash
uname -a
which sudo
```

Expected:

* Kernel shows `Linux ... WSL2`
* `sudo` exists at `/usr/bin/sudo`

If this fails, **stop here**.

---

## `aws-cli-profiles.md`

### Why AWS CLI Is the Authority

MCP does **not manage credentials**.

All execution authority comes from:

* AWS CLI
* IAM policies
* active AWS profile

If AWS CLI identity is wrong, MCP will be wrong.

---

### Install AWS CLI v2

```bash
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o awscliv2.zip
unzip awscliv2.zip
sudo ./aws/install
```

Verify:

```bash
aws --version
```

---

### Create a Dedicated MCP Profile (Non-Negotiable)

Do **not** reuse:

* customer SSO profiles
* corporate default profiles
* admin convenience profiles

Create a **clean, purpose-built profile**:

```bash
aws configure --profile mcp01
```

Provide:

* Access Key ID
* Secret Access Key
* Region (example: `ap-south-1`)
* Output format: `json`

---

### Validate Identity (Ground Truth)

```bash
aws sts get-caller-identity --profile mcp01
```

This command must succeed **before continuing**.

---

### Make the Profile Explicit

Set the profile at shell level:

```bash
export AWS_PROFILE=mcp01
export AWS_REGION=ap-south-1
```

Persist it:

```bash
echo 'export AWS_PROFILE=mcp01' >> ~/.bashrc
echo 'export AWS_REGION=ap-south-1' >> ~/.bashrc
source ~/.bashrc
```

Verify:

```bash
env | grep AWS
```

Only these should appear:

```
AWS_PROFILE=mcp01
AWS_REGION=ap-south-1
```

---

## `amazon-q-cli-setup.md`

### What Amazon Q CLI Actually Is

Amazon Q CLI is:

* a **reasoning client**
* not an execution engine
* not a credential store

It:

* reads AWS context
* asks permission
* invokes MCP tools

---

### Install Amazon Q CLI (Linux)

```bash
curl --proto '=https' --tlsv1.2 -sSf \
https://desktop-release.codewhisperer.us-east-1.amazonaws.com/latest/q-x86_64-linux-musl.zip \
-o q.zip

unzip q.zip
cd q
chmod +x install.sh
sudo ./install.sh
```

Ensure PATH stability:

```bash
echo 'export PATH=$PATH:/usr/local/bin:$HOME/.local/bin' >> ~/.bashrc
source ~/.bashrc
```

Verify:

```bash
which q
q --version
```

---

### Authenticate Amazon Q

```bash
q login
```

Choose:

* **Use for Free with Builder ID** (or org login if applicable)

This creates a **session-bound token**, not permanent credentials.

---

### Validate Q + AWS Alignment

Start chat:

```bash
q chat
```

Ask:

```
Who am I authenticated as in AWS?
```

Approve execution (`t`).

Expected:

* Correct AWS account
* Correct IAM user
* No SSO errors

This confirms **Q is bound to your AWS CLI context**.

---

## `troubleshooting-auth-sso.md`

### Why SSO Is Dangerous for MCP Labs

AWS SSO:

* injects environment variables
* caches tokens
* silently overrides profiles

This causes:

* wrong account usage
* expired token failures
* unpredictable behavior

---

### Detect SSO Contamination

```bash
env | grep AWS
```

Red flags:

* `AWS_PROFILE=<customer-profile>`
* `AWS_SDK_LOAD_CONFIG`
* unexpected regions

---

### Remove SSO Completely

1. Remove from shell config:

```bash
nano ~/.bashrc
```

Delete any:

```bash
export AWS_PROFILE=<old-sso-profile>
```

2. Remove cached SSO tokens:

```bash
rm -rf ~/.aws/sso
```

3. Restart WSL:

```powershell
wsl --shutdown
```

4. Reopen Ubuntu and revalidate:

```bash
aws sts get-caller-identity
```

---

### Golden Rule (Non-Negotiable)

> **If `aws sts get-caller-identity` is wrong, MCP is wrong.
> Fix AWS CLI first. Always.**

---

## End of Setup Section — Why This Matters

Most MCP demos fail **not because MCP is immature**,
but because environments are messy.

This setup ensures:

* predictable behavior
* safe AI execution
* enterprise-grade hygiene

From here onward:

* failures are meaningful
* outcomes are trustworthy
* labs reflect real-world conditions

---

### Next Section

 `/02-mcp-servers`
Where we move from **environment correctness** to **capability selection**.


