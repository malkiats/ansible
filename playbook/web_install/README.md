# Web Install Scenario

This directory contains a simple service deployment example for Apache on hosts in the `webserver` inventory group.

## Files

- `hosts`: inventory file used by the example.
- `site.yml`: playbook that installs dependencies and configures Apache.
- `roles/`: notes about organizing the same kind of deployment with reusable roles.

## What `site.yml` Does

The playbook demonstrates a small but realistic workflow:

- installs `epel-release`
- installs SELinux Python bindings
- checks SELinux status with `getenforce`
- installs `httpd`
- starts and enables the Apache service

## Run The Example

```bash
ansible-playbook -i hosts site.yml
```

## Notes

- The playbook uses package and service names common on RHEL or CentOS systems.
- Review the inventory before running in your environment.
- A production version of this workflow would normally move package, template, and service logic into roles.
