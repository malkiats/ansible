# Deep Dive 1: Bootstrap Workflow

This exercise focuses on a small multi-group bootstrap playbook that applies different tasks to different host groups.

## File

- `bootstrap.yml`: the main exercise playbook.

## What The Playbook Covers

### All hosts

- add `ansible.xyzcorp.com 169.168.0.1` to `/etc/hosts`
- install `elinks`
- create the `xyzcorp_audit` user
- copy `/home/ansible/motd` to `/etc/motd`
- copy `/home/ansible/issue` to `/etc/issue`

### `network` hosts

- install `nmap-ncat`
- create the `xyzcorp_network` user

### `sysadmin` hosts

- copy `/home/ansible/scripts.tgz` to `/mnt/storage/`

## Why This Exercise Matters

This example shows how one playbook can:

- target different inventory groups
- apply shared tasks first
- apply role-specific tasks afterward
- combine package, user, file, and line management in a single workflow

## Before You Run It

- make sure the inventory contains `all`, `network`, and `sysadmin` hosts as expected
- confirm the source files exist on the control node
- review privileges because the playbook writes to system paths
