---
name: check-credentials
description: Scan pull request diffs for hardcoded credentials, secrets, API keys, and sensitive values. Trigger phrases: "review PR", "check for secrets", "audit credentials", "scan for hardcoded keys".
---

# check-credentials

Scan pull request diffs for hardcoded credentials, secrets, and sensitive values.

## Steps
1. Scan every added/modified line for patterns:
   - AWS access keys: `AKIA`, `ASIA`, `AROA`
   - Token prefixes: `sk-`, `ghp_`, `xoxb-`, `xoxp-`
   - Variable names containing `secret`, `password`, `passwd`, `token`, `key`, `credential` assigned to string literals
   - AWS account IDs (12-digit numbers in ARNs)
   - Private keys (PEM headers: `-----BEGIN`)
2. Flag any match as a **blocking** finding with file path, line number, and pattern matched.
3. Check `.gitignore` excludes `.env*`, `*.pem`, `*.key`, `secrets.*`.
4. Verify GitHub Actions workflows reference secrets as `${{ secrets.VAR }}`, not plaintext.
5. If clean, confirm: "No hardcoded credentials found."

## Rules
- PLAINTEXT values in CodeBuild env vars are a **blocker** — must use `PARAMETER_STORE` or `SECRETS_MANAGER` type.
- Any hardcoded AWS account ID in source (outside comments/docs) is a **blocker**.
- Warn (non-blocking) on string literals >20 chars that look like tokens.
