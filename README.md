# RamenDR TechDemo Config As Code

Ansible Automation Platform config-as-code for the **RamenDR TechDemo** validated pattern.

Consumed by AGOF to configure AAP Controller (Redis on RHEL VMs, Route 53 failover, etc.).

## What It Creates in AAP

| Resource | Name | Purpose |
|----------|------|---------|
| Organization | RamenDR TechDemo | Top-level org for all resources |
| Project | RamenDR TechDemo Ansible Collection | SCM project for [rhvp.ramendr_techdemo](https://github.com/validatedpatterns-demos/ramendr-techdemo-config-as-code) |
| Inventory | RamenDR VM Inventory | Placeholder — playbooks discover hosts dynamically |
| Credential | vm_ssh_credential | Machine credential for SSH to RHEL VMs |
| Job Template | Discover and Install Redis | Discovers Redis VM LB, registers RHEL, installs Redis |
| Job Template | Setup AWS Route53 Failover | Route 53 failover CNAME records |

## Secrets

On OpenShift, AGOF uses **hub cluster Vault** (`doAutoHubVaultConfig`), following the same pattern as [ansible-edge-gitops-hmi-config-as-code](https://github.com/validatedpatterns-demos/ansible-edge-gitops-hmi-config-as-code/blob/main/controller_config.yml): empty `agof_vault.yml`, `Hashivault` credential, and `controller_credential_input_sources` after job templates/schedules.

Load secrets from the pattern repo:

```bash
./pattern.sh make load-secrets
```

Use `values-secret.yaml` (from `ramendr-techdemo/values-secret.yaml.template`).

| AAP credential | Vault path | Keys |
|----------------|------------|------|
| vm_ssh_credential | `global/vm-ssh` | `username`, `privatekey` |
| hub_k8s_credential | `hub/hub-k8s` | `host`, `token` |
| rhsm_credential | `hub/rhsm` | `username`, `password` |
| aws_credential | `hub/aws` | `aws_access_key_id`, `aws_secret_access_key` |

`admin_password` is discovered from the AAP Operator `aap-admin-password` secret (not in Vault).

Do **not** use `agof-vault-file` / `vaultFileKey`; set `agof.vaultFileKey: ""` in pattern overrides.

## Pattern wiring

In `ramendr-techdemo/overrides/values-aap-config-rdr.yaml`:

```yaml
agof:
  iac_repo: https://github.com/validatedpatterns-demos/ramendr-techdemo-config-as-code.git
  iac_revision: main
  vaultFileKey: ""
  doAutoHubVaultConfig: true
```

## Getting started

1. Concepts follow the [Ansible Edge GitOps](https://github.com/validatedpatterns/ansible-edge-gitops) validated pattern.
2. Deploy via [ramendr-techdemo](https://github.com/validatedpatterns-sandbox/ramendr-techdemo) (or your fork), which references this repo as the AGOF IAC source.
