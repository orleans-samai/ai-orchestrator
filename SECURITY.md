# Security policy

## Intended use

AI Orchestrator coordinates third-party coding agents on a local Linux workspace. It is a workflow aid, not a security boundary.

Before running it:

- review the target repository's current changes;
- remove production credentials and sensitive data from agent-visible files;
- configure each provider using least privilege;
- maintain tested backups;
- understand every approval request.

Never use dangerous permission-bypass options merely to make the workflow unattended. The orchestrator intentionally requires human approval at critical transitions.

## Reporting a vulnerability

Do not disclose credentials, private repository contents, or exploit details in a public issue. Use the repository owner's private vulnerability-reporting channel when one is available.

Include the affected version, environment, reproduction steps, impact, and a proposed mitigation if known.

## Provider security

Antigravity, Codex, and Claude Code have independent authentication, permissions, data-handling terms, and security controls. Review each provider's current official documentation before use.

