# Advanced Playbook Syntax

This directory contains focused examples for Ansible execution control, task selection, error handling, and secret management.

## Files

- `async.yml`: asynchronous task execution with `async` and `poll`.
- `blck_rescue_always.yml`: block, rescue, and always behavior.
- `delegate.yml`: `delegate_to` and `local_action` style execution.
- `ignore_error.yml`: handling failures with `ignore_errors`, `changed_when`, and `failed_when` patterns.
- `run_once.yml`: tasks that should execute one time for a play.
- `tags.yml`: tag-based selective execution.
- `vault.yml`: working with sensitive values using Ansible Vault.

## Typical Use Cases

- long-running operations that should not block the whole play
- partial runs during testing or controlled maintenance windows
- error-handling flows where cleanup or rescue logic matters
- tasks that must execute on the controller instead of the target host
- secret handling without placing sensitive values in plain text

## Tag Examples

```bash
ansible-playbook tags.yml --tags software
ansible-playbook tags.yml --skip-tags software
ansible-playbook tags.yml -t software,files
```

## Notes

- These examples are best treated as focused syntax references rather than end-to-end deployment playbooks.
- Review module names and indentation before using a file unchanged in production.
