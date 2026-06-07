# Overview

> A practical AI infrastructure project documenting the deployment of Hermes Agent on AWS EC2, integration with WhatsApp, and troubleshooting real-world cloud and LLM deployment challenges.

As part of my AI Engineering learning journey, I deployed Hermes Agent on an AWS EC2 Ubuntu instance and connected it to WhatsApp, allowing me to interact with an AI agent remotely through my phone.
This project provided hands-on experience with cloud infrastructure, Linux, SSH, AWS IAM, Amazon Bedrock, Google Gemini API integration, tmux session management, and agent deployment troubleshooting.

# Architecture

WhatsApp → Hermes Gateway → LLM Provider (Gemini / Bedrock) → AWS EC2 (Ubuntu)

Objective: To run Hermes continuously on a cloud server so it could be accessed through WhatsApp without requiring my laptop to remain online.

# Skills Demonstrated

- AWS EC2
- Linux Administration
- SSH
- IAM Roles
- Amazon Bedrock
- Google Gemini API
- AI Agents
- Process Management (tmux)
- Cloud Troubleshooting
- Security Groups
- WhatsApp Automation
- Infrastructure Debugging

# Installation Guide

## Prerequisites

Before starting, ensure you have:

- AWS Account
- Ubuntu EC2 Instance
- SSH Key Pair (.pem)
- Google AI Studio API Key (or Bedrock access)
- Hermes Agent

---

## AWS Infrastructure Setup

### 1. Create EC2 Instance

- AWS EC2
- Ubuntu Server 24.04 LTS
- t3.small (Free Tier) or higher
- Create and download a PEM key pair
- Allow SSH (Port 22) in Security Groups

### 2. Configure Security Group

Inbound Rules:

| Type | Port | Source |
|--------|--------|--------|
| SSH | 22 | My IP |

### 3. Connect to EC2

Connect from your local terminal or PowerShell:

```bash
ssh -i "your-key.pem" ubuntu@<EC2-PUBLIC-IP>
```

Verify connection:

```bash
whoami
```

Expected output:

```text
ubuntu
```

---

## Update Ubuntu

```bash
sudo apt update
sudo apt upgrade -y
```

---

## Install tmux

```bash
sudo apt install tmux -y
```

Verify installation:

```bash
tmux -V
```

# Hermes Installation

## Clone Hermes

```bash
git clone <hermes-repository-url>
cd hermes-agent
```

---

## Install Dependencies

```bash
npm install
```

or follow the official Hermes installation guide. (https://hermes-agent.nousresearch.com/docs/getting-started/quickstart#1-install-hermes-agent)

---

## Configure Provider

Select a provider:

```bash
hermes model
```

Options tested:

- Amazon Bedrock
- Google Gemini

---

## Configure Google Gemini

Obtain an API key from Google AI Studio.

During setup:

```text
GOOGLE_API_KEY=YOUR_API_KEY
```

Default Base URL:

```text
https://generativelanguage.googleapis.com/v1beta
```

---

## Configure WhatsApp

```bash
hermes whatsapp
```

Choose:

```text
2. Personal number (self-chat)
```

Pair using the QR code.

---

## Start Hermes Gateway

```bash
hermes gateway
```

Expected output:

```text
[WhatsApp] Bridge ready
```

# Running Hermes with tmux

Create a session:

```bash
tmux new -s hermes
```

Start gateway:

```bash
hermes gateway
```

Detach safely:

```text
Ctrl+B
D
```

Reattach later:

```bash
tmux attach -t hermes
```

List sessions:

```bash
tmux ls
```

Stop gateway:

```bash
Ctrl+C
```

Learning:

- Ctrl+C stops Hermes
- Ctrl+B then D keeps Hermes running

# Major Challenges Encountered

# 1. SSH Connection Timeouts

Issue:

- Lost SSH access after several days.
- Connection timed out despite EC2 being in Running state.

Root Cause:

- EC2 Security Group SSH rule was restricted to my previous public IP address.

Solution:

- Updated inbound rule for port 22 to my current public IP.
- Successfully restored SSH connectivity.

Learning:

- SSH access depends on Security Group rules, not just EC2 instance status.

# 2. Installing Hermes Agent

Issue:

- Attempted Linux installation commands from Windows PowerShell.
- Commands such as bash were not recognized.

Solution:

- Connected to Ubuntu EC2 first.
- Executed installation commands directly inside Linux.

Learning:

- Linux commands must be executed inside the Linux environment, not Windows PowerShell.

# 3. Amazon Bedrock Authentication

Issue:

- Hermes could not detect AWS credentials.

Root Cause:

- EC2 instance did not have an IAM Role attached.

Solution:

- Created and attached an IAM Role for Bedrock access.
- Verified credentials using:

  aws sts get-caller-identity

Learning:

- IAM Roles are the preferred method for EC2 authentication.

# 4. Bedrock Model Access Errors

Issue:

Claude Sonnet models returned:

- ResourceNotFoundException

Root Cause:

- Anthropic requires first-time users to submit use case details before model access.

Solution:

- Submitted Anthropic use case information through Bedrock.

Learning:

- Some Bedrock providers require additional onboarding steps before model invocation.

# 5. Bedrock Quota Limitations

Issue:

Received:

- ThrottlingException
- Too many tokens per day

Root Cause:

- New AWS accounts often have limited Bedrock quotas.

Solution:

- Switched to Google Gemini API as the inference provider.

Learning:

- Quotas and service limits are common deployment considerations.

# 6. Gemini API Integration

Issue:

- Needed an alternative LLM provider.

Solution:

- Configured Gemini API key within Hermes.
- Successfully switched inference provider.

Learning:

- Hermes supports multiple providers and can be reconfigured without rebuilding infrastructure.

# 7. WhatsApp Integration

Issue:

- Needed a user-friendly interface for interacting with Hermes.

Solution:

- Connected Hermes using WhatsApp self-chat mode.
- Successfully communicated with Hermes through WhatsApp.

Learning:

- Messaging platforms can serve as lightweight frontends for AI agents.

# 8. Process Persistence Problem

Issue:

- Hermes stopped responding after SSH sessions disconnected.

Root Cause:

- Gateway process terminated when terminal session closed.

Solution:

- Installed and configured tmux.
- Ran Hermes Gateway inside a tmux session.

Learning:

- Long-running services should not depend on active SSH sessions.

# 9. Understanding tmux

Common Mistake:

- Confusing Ctrl+C with tmux detach.

Result:

- Ctrl+C terminated Hermes Gateway.

Correct Workflow:

  Start session:
  
  tmux new -s hermes
  
  Run Hermes:
  
  hermes gateway
  
  Detach safely:
  
  Ctrl+B
  D

Reconnect later:

  tmux attach -t hermes

Learning:

- Detaching preserves running processes.
- Ctrl+C terminates them.

# Final Outcome

Successfully deployed a cloud-hosted Hermes Agent accessible through WhatsApp.

Capabilities Demonstrated:

- Interact with an AI agent remotely through WhatsApp
- Query LLMs from a cloud-hosted environment
- Experiment with AI engineering workflows
- Explore repository analysis and technical research use cases
- Deploy and manage agent infrastructure on AWS

The agent remains available even when my laptop is offline because it runs continuously on AWS EC2.

# Key Takeaways

- Learned practical Linux administration.
- Understood AWS networking and security groups.
- Worked with IAM roles and Bedrock authentication.
- Experienced real-world API quota and access issues.
- Learned process management using tmux.
- Successfully deployed and maintained a cloud-hosted AI agent.

Prior to this project, I mainly focused on building AI applications and models.

This deployment experience highlighted that AI Engineering also involves infrastructure, authentication, cloud services, networking, process management, quotas, monitoring, and operational troubleshooting.

Building the model is only one part of the solution. Running it reliably in production is a different challenge entirely.
