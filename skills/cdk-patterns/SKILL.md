# cdk-patterns

Review CDK code for anti-patterns, security misconfigurations, and best-practice violations.

## Triggers
- PR changes files under `cdk/`, `infra/`, `lib/`, or any `*.ts`/`*.py` file importing from `aws-cdk-lib`

## Steps
1. **ARM64**: check Lambda uses `architecture: Architecture.ARM_64` and ECS uses `cpuArchitecture: CpuArchitecture.ARM64`. x86 without justification = warning.
2. **Public resources**: flag S3 bucket with `publicReadAccess: true` or missing `BLOCK_ALL`. Flag Lambda with `authType: FunctionUrlAuthType.NONE`. Both are **blockers**.
3. **Encryption**: S3, RDS, DynamoDB must have encryption enabled. Missing = warning.
4. **Removal policy**: production stacks must not have `removalPolicy: RemovalPolicy.DESTROY` on stateful resources.
5. **Secrets**: no `SecretValue.unsafePlainText()`. Must use `SecretValue.secretsManager()` or SSM.
6. **CloudFront**: S3 origins must use OAC (`S3BucketOrigin.withOriginAccessControl()`), not OAI or public bucket.
7. **Outputs**: no sensitive values in `CfnOutput`.

## Rules
- Always ARM64. Always encrypt. Never public.
- Secrets Manager > SSM > env vars > hardcoded.
- One stack = one concern.
