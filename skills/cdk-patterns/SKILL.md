---
name: cdk-patterns
description: Review CDK code for anti-patterns, security misconfigurations, and AWS best-practice violations. Trigger phrases: "review PR", "CDK review", "check infrastructure", "audit CDK".
---

# cdk-patterns

Review CDK code for anti-patterns, security misconfigurations, and best-practice violations.

## Steps
1. **ARM64**: Lambda must use `architecture: Architecture.ARM_64`; ECS must use `cpuArchitecture: CpuArchitecture.ARM64`. x86 without justification = warning.
2. **Public resources**: S3 bucket with `publicReadAccess: true` or missing `BLOCK_ALL` = **blocker**. Lambda with `authType: FunctionUrlAuthType.NONE` = **blocker**.
3. **Encryption**: S3, RDS, DynamoDB must have encryption enabled. Missing = warning.
4. **Removal policy**: production stacks must not have `removalPolicy: RemovalPolicy.DESTROY` on stateful resources (S3, RDS, DynamoDB).
5. **Secrets**: no `SecretValue.unsafePlainText()`. Must use `SecretValue.secretsManager()` or SSM.
6. **CloudFront**: S3 origins must use OAC (`S3BucketOrigin.withOriginAccessControl()`), not OAI or public bucket.
7. **Outputs**: no sensitive values (passwords, keys) in `CfnOutput`.

## Rules
- Always ARM64. Always encrypt. Never public.
- Secrets Manager > SSM > env vars > hardcoded.
- One stack = one concern.
