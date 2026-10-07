---
name: iam-policy-review
description: Review IAM policies, roles, and trust relationships for least-privilege violations. Trigger phrases: "review PR", "check IAM", "audit permissions", "least privilege review".
---

# iam-policy-review

Review IAM policies, roles, and trust relationships for least-privilege violations.

## Steps
1. Find all IAM policy statements (inline, managed, CDK `grant*` calls, CFN `AWS::IAM::Policy`).
2. For each `Effect: Allow` statement, check:
   - **Action wildcard** (`*` or `service:*`): **blocking** unless resource is also `*` AND justification is documented.
   - **Resource `*`** combined with write/delete actions: **blocking**.
   - `sts:AssumeRole` with `Principal: "*"`: **blocker** — must have a condition.
3. Check trust policies: `Principal` must be a specific service or ARN, never `"*"`.
4. Flag `iam:PassRole` without a condition on `iam:PassedToService`.
5. Check CDK: prefer `grant*` methods over manual `addToPolicy` calls.
6. Verify no `AdministratorAccess` managed policy attached to anything except a break-glass role.

## Rules
- Least privilege: only the actions needed, only on the resources needed.
- `*` on resource requires explicit justification in PR description.
- Cross-account trust: must have `aws:PrincipalOrgID` or explicit account condition.
