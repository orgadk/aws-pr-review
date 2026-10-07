# buildspec-rules

Review CodeBuild buildspec.yml files for correctness, safety, and reliability.

## Triggers
- PR changes `buildspec*.yml`, `buildspec*.yaml`, or `appspec.yml`

## Steps
1. **CODEBUILD_SRC_DIR**: every `cd` command must use `$CODEBUILD_SRC_DIR/path`. A bare `cd somedir` does NOT persist between commands. Flag any `cd` without `$CODEBUILD_SRC_DIR` as a **blocker**.
2. **Error swallowing**: flag any `|| echo` or `|| true` on deployment or build commands. Silent failures leave old versions running. **Blocking**.
3. **Env var types**: `PLAINTEXT` values in `environment/variables` are a **blocker**. Must use `PARAMETER_STORE` or `SECRETS_MANAGER` type for secrets.
4. **ARM64**: if the pipeline builds Docker images, verify `--platform linux/arm64` is specified. Missing = warning.
5. **Phase hygiene**: `install` = tools only; `pre_build` = auth/setup; `build` = build; `post_build` = push/deploy. Flag logic in wrong phases.
6. **Artifacts**: verify `base-directory` is set when uploading artifacts.
7. **Cache**: if `node_modules/` appears in build commands, verify a `cache/paths` block exists.

## Rules
- Every `cd` needs `$CODEBUILD_SRC_DIR`. No `|| echo` on critical steps. No PLAINTEXT secrets.
