# Application Role README Template

## Purpose

This role deploys or updates one application component with predictable, repeatable steps.

## What This Role Manages

Document the exact scope.

- application package or artifact
- configuration files
- service unit
- health check endpoint

## What This Role Does Not Manage

Be explicit about boundaries.

- database schema migration
- external DNS updates
- load balancer creation

## Supported Platforms

- RHEL 8
- RHEL 9
- Ubuntu 22.04

## Required Variables

```yaml
application_name: myapp
application_version: 1.2.3
application_artifact_url: https://repo.example.com/myapp.tar.gz
```

## Optional Variables

```yaml
application_user: myapp
application_group: myapp
application_install_dir: /opt/myapp
application_service_name: myapp
application_healthcheck_url: http://localhost:8080/health
```

## Secret Variables

Store these outside plain-text vars.

```yaml
vault_application_api_token: vault-managed
vault_database_password: vault-managed
```

## Execution Flow

List the high-level sequence.

1. validate required variables
2. create users and directories
3. download or copy the release artifact
4. render configuration from templates
5. enable or restart the service only when needed
6. run a simple post-deploy validation

## Idempotency Notes

Explain how the role avoids unnecessary changes.

- package or artifact tasks use state-aware modules
- templates notify handlers only on change
- service state is declared explicitly

## Example Playbook Usage

```yaml
- hosts: app
  become: true
  roles:
    - role: application
      vars:
        application_name: billing-api
        application_version: 2026.06.1
        application_artifact_url: https://repo.example.com/billing-api-2026.06.1.tar.gz
```

## Validation Checklist

- syntax check passes
- ansible-lint passes
- role runs successfully in development
- second run is idempotent
- health check succeeds after deployment

## Rollback Considerations

Document how to recover from failure.

- keep previous release artifact available
- preserve rendered config backups if needed
- define whether service restart is immediate or controlled

## Ownership

Document who maintains this role and where to open changes.
