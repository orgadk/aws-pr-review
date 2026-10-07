# check-credentials

Scan pull request diffs for hardcoded credentials, secrets, and sensitive values.

## Triggers
- PR review requested
- Files changed include `.env`, config, YAML, JSON, source code

## Steps
1. Scan every added/modified line for patterns:
   - API keys: `AKIA`, `sk-`, `ghp_`, `xoxb-`, `xoxp-`
   - Generic secrets: variable names containing `secret`, `password`, `passwd`, `token`, `key`, `credential` assigned to string literals
   - AWS account IDs (12-digit numbers in ARNs)
   - Private keys (PEM headers)
2. Flag any match as a **blocking** finding with the file path, line number, and pattern matched.
3. Check that `.gitignore` excludes `.env*`, `*.pem`, `*.key`, `secrets.*`.
4. Verify no `secrets:` blocks in GitHub Actions workflows use plaintext values — must reference `${{ secrets.VAR }}`.
5. If clean, confirm explicitly: "No hardcoded credentials found."

## Rules
- PLAINTEXT values in CodeBuild env vars are a **blocker** — must use `PARAMETER_STORE` or `SECRETS_MANAGER` type.
- Any hardcoded AWS account ID in source (not in comments/docs) is a **blocker**.
- Warn (non-blocking) on any string literal longer than 20 chars that looks like a token.
