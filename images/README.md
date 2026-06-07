# Project Screenshots

This folder contains screenshots documenting the deployment, configuration, troubleshooting, and operation of Hermes Agent on AWS EC2 with WhatsApp integration.

---

## 1. SSH Security Group Configuration

**File:** `ssh-security-group-rule.jpeg`

Demonstrates updating AWS Security Group inbound rules to allow SSH access from the current public IP address.

**Key Learning:**
- EC2 can remain in a Running state while SSH access is blocked.
- Security Group rules directly control network access.

---

## 2. IAM Role Attachment

**File:** `iam-role-attached.jpeg`

Shows the IAM role successfully attached to the EC2 instance for Amazon Bedrock access.

**Key Learning:**
- EC2 should use IAM Roles instead of hardcoded AWS credentials.
- Required for secure Bedrock authentication.

---

## 3. Anthropic Use Case Submission

**File:** `anthropic-use-case-submission.jpeg`

Documents the onboarding process required before accessing Anthropic Claude models through Amazon Bedrock.

**Key Learning:**
- Some Bedrock model providers require additional approval steps.
- Model access may fail even when IAM permissions are correct.

---

## 4. WhatsApp Integration

**File:** `whatsapp-hermes-chat.jpeg`

Shows successful communication with Hermes Agent through WhatsApp.

**Key Learning:**
- WhatsApp can act as a lightweight frontend for AI agents.
- Messages are routed through Hermes Gateway to the configured LLM provider.

---

## 5. tmux Session Persistence

**File:** `detached-tmux-session.jpeg`

Demonstrates detaching from a tmux session while keeping Hermes running in the background.

**Key Learning:**
- Long-running services should not depend on active SSH sessions.
- tmux enables persistent terminal processes.

---

## 6. Hermes Gateway Running

**File:** `hermes-gateway-running.jpeg`

Shows Hermes Gateway already running and refusing to launch a duplicate instance.

**Key Learning:**
- Verifies successful background execution.
- Confirms process persistence after SSH disconnects.

---

## Summary

These screenshots capture the complete deployment journey:

1. AWS EC2 setup
2. Security Group troubleshooting
3. IAM configuration
4. Bedrock onboarding
5. WhatsApp integration
6. Process persistence with tmux
7. Continuous Hermes Gateway operation

Together they document the practical challenges encountered while deploying a cloud-hosted AI agent.
