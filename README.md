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
