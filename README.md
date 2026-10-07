# aws-pr-review

Claude Code plugin — AWS PR review skills.

## Skills

| Skill | What it checks |
|---|---|
| `check-credentials` | Hardcoded secrets, API keys, tokens in code |
| `no-over-engineering` | Unnecessary complexity, gold-plating |
| `iam-policy-review` | Overly permissive IAM policies, wildcards |
| `cdk-patterns` | CDK anti-patterns, x86 defaults, public resources |
| `buildspec-rules` | CodeBuild buildspec correctness and safety |

## Install

```
/plugin install aws-pr-review@cc-plugin-marketplace
```
