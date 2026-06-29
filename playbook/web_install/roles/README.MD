# Role-Oriented Project Notes

This note shows how a playbook can be decomposed into reusable roles instead of embedding all logic directly in one file.

## Example Layout

```text
site.yml
roles/
  common/
    files/
    templates/
    tasks/
    handlers/
    vars/
    defaults/
    meta/
  webservers/
    files/
    templates/
    tasks/
    handlers/
    vars/
    defaults/
    meta/
```

## Example Playbook

```yaml
---
- hosts: web
  roles:
    - common
    - webservers
```

## Why Use Roles

- playbooks stay small and readable
- shared logic can be reused across projects
- variables, handlers, templates, and tasks remain organized by responsibility
- testing and maintenance become easier

## Notes

- `common` is a good place for baseline configuration shared across many hosts.
- `webservers` should contain only web-tier specific logic.
- For third-party reusable roles, review Ansible Galaxy: `https://galaxy.ansible.com`
