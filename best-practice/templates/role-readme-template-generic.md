# Role Name

## Purpose

Describe the single responsibility of this role in one short paragraph.

Examples:

- install and configure nginx
- create baseline operating system settings
- deploy an application service

## Supported Environments

List the environments where this role is expected to run.

- development
- staging
- production

## Inventory Targets

Document which inventory groups commonly use this role.

- `web`
- `app`
- `common`

## Variables

### Required variables

List variables that must be supplied by the playbook, inventory, or pipeline.

```yaml
application_name: myapp
```

### Optional variables

List tunable values with defaults.

```yaml
service_enabled: true
service_state: started
```

## Secrets

Document any variables that must come from Vault or another secret manager.

```yaml
vault_application_token: vault-managed
```

## Dependencies

Describe any role dependencies, packages, collections, or external services.

- community.general
- firewall role
- load balancer API access

## Tasks Performed

Summarize the important actions this role takes.

- install packages
- render configuration
- enable and start service
- notify handlers when config changes

## Handlers

Document which handlers may run.

- restart nginx
- reload systemd

## Example Usage

```yaml
- hosts: web
  become: true
  roles:
    - role: role_name
      vars:
        application_name: myapp
```

## Validation

Describe how to test this role safely.

- run syntax check
- run ansible-lint
- test in development inventory
- verify idempotency with a second run

## Notes

Include assumptions, known limitations, and operational guidance.
