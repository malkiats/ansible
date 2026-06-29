# Inventory Layout

This scaffold separates environments so deployments do not require editing playbooks.

## Included Environments

- `dev/hosts.yml`
- `staging/hosts.yml`
- `prod/hosts.yml`

## Usage

```bash
ansible-playbook -i inventory/dev/hosts.yml playbooks/site.yml
ansible-playbook -i inventory/staging/hosts.yml playbooks/site.yml
ansible-playbook -i inventory/prod/hosts.yml playbooks/site.yml
```

Keep inventory data free of secrets. Store passwords, tokens, and other sensitive material in Vault or an approved secret manager.
