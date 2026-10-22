# STACKIT KMS wrapper

This module implements envelope encryption with STACKIT KMS. The configured
KMS key encrypts only the per-message data-encryption key; payload encryption
remains local to the caller.

Required configuration:

- `project_id`
- `region`
- `key_ring_id`
- `key_id`

`key_version` is optional. When omitted, the wrapper selects the latest usable
key version and records that version in every wrapped blob so older data remains
decryptable after key rotation.

## SKE workload identity

Use these additional settings for an SKE workload identity:

```hcl
workload_identity_federation = "true"
service_account_email        = "vault@sa.stackit.cloud"
federated_token_file         = "/var/run/secrets/stackit.cloud/serviceaccount/token"
```

The token path must point to the projected token injected for the pod. Workload
identity cannot be combined with a static service-account key or bearer token.
The wrapper does not include the service-account email or token path in returned
configuration metadata.

Static service-account keys and tokens remain supported for compatibility and
local testing, but workload identity is the recommended Kubernetes deployment
mode.
