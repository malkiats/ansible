# nginx Role

## Purpose

Install and configure nginx for hosts in the web tier.

## Variables

```yaml
nginx_listen_port: 80
nginx_server_name: example.internal
application_upstream_port: 8080
```

## What This Role Manages

- nginx package installation
- main reverse proxy configuration
- service enable and restart behavior

## Validation

- syntax check the playbooks that include this role
- run ansible-lint
- verify the rendered configuration
- rerun the playbook to confirm idempotency
