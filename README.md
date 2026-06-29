# Ansible Lab Notes and Playbook Reference

This repository is a compact Ansible practice workspace. It mixes ad-hoc command notes, standalone playbook examples, and small scenario-based labs for common administration tasks.

Most examples target Linux hosts with an `ansible` user and privilege escalation. Several samples assume RHEL or CentOS style package and service names such as `yum`, `dnf`, `httpd`, and `epel-release`.

## Repository Layout

For project design, maintainability, and operational guidance, see `best-practice/README.md`, `best-practice/sample-production-ansible/README.md`, `best-practice/templates/role-readme-template-generic.md`, and `best-practice/templates/role-readme-template-application.md`.

### Ad-hoc command and setup notes

These top-level files are quick references for common Ansible operations:

- `install_config`: initial controller setup, inventory basics, passwordless SSH, and first connectivity checks.
- `static_inventory`: example static inventory with groups such as `control`, `web`, and `haproxy`.
- `module_shell`: common modules, package installs, fact gathering, and `command` or `shell` examples.
- `soft_install`: package management examples using `package`, `yum`, and `apt`.
- `daemons_services`: service start, stop, enable, and disable examples.
- `user_groups`: user and group management commands.
- `file_manipulation`: file creation, copy, archive, unarchive, line editing, downloads, and a few lab-style tasks.
- `epel_repo`: EPEL repository installation and verification steps.
- `facts.yml`: a standalone playbook that writes gathered facts such as hostname and controller IP data into a file on managed hosts.
- `best-practice/README.md`: guidance for building maintainable, secure, reusable, and production-ready Ansible automation.
- `best-practice/sample-production-ansible/README.md`: a production-style sample Ansible project scaffold.
- `best-practice/templates/role-readme-template-generic.md`: a general README template for reusable roles.
- `best-practice/templates/role-readme-template-application.md`: a README template for application deployment roles.

### Structured playbook examples

- `playbook/`: general playbook examples, sample tasks, and a small web installation scenario.
- `playbook/ad_hoc_to_playbook/`: playbook equivalents for several top-level ad-hoc command notes.
- `deepdive/`: deeper scenario exercises focused on copying files, user management, variables, facts, handlers, loops, and templates.
- `Playbook_Syntax/`: focused examples for advanced execution behavior such as async runs, delegation, tags, vault usage, and error handling.

## Detailed Contents

### `playbook/`

This directory contains reusable examples for writing and organizing playbooks.

- `linuxpatch.yml`: rolling patching example with serial execution, process checks, package updates, and conditional reboot handling.
- `playbook_sample_web`: note file related to web playbook examples.
- `task_in_play`: notes about tasks embedded directly inside a play.
- `sample/`: a learning set that covers most day-to-day playbook features.
- `ad_hoc_to_playbook/`: playbook-form examples for controller bootstrap, module usage, package management, services, files, users, and EPEL setup.
- `web_install/`: installs Apache and supporting packages on a `webserver` inventory group.

#### `playbook/sample/`

The sample area includes focused examples for:

- `play.yml` and `sample.yml`: basic playbook structure.
- `vars.yml` and `vars_play.yml`: variable definitions, lists, and dictionaries.
- `template.yml`, `config.j2`, and `httpd.conf.j2`: Jinja2 templating.
- `facts.yml`: fact usage.
- `cond.yml`: conditional task execution.
- `loop.yml` and `loop2.yml`: loop patterns.
- `handler.yml`: handlers and notify behavior.
- `delegate.yml`: delegating tasks to other hosts or the controller.
- `async.yml`: asynchronous task execution.
- `run-once.yml`: tasks that should execute only once per play.
- `serial.yml`: rolling updates across hosts.
- `tags.yml`: selective execution with tags.
- `error1.yml` and `error2.yml`: error-handling examples.
- `secret.yml`: sensitive data handling patterns.
- `conclusion.yml`: end-of-topic wrap-up example.

#### `playbook/ad_hoc_to_playbook/`

This area converts command-line notes into playbooks so you can compare ad-hoc usage with repeatable automation:

- `control_node_bootstrap.yml`: localhost-oriented controller preparation example.
- `module_shell_examples.yml`: connectivity, facts, package, command, and shell tasks.
- `package_management.yml`: install and remove packages with playbooks.
- `service_management.yml`: declare service state and boot-time enablement.
- `file_management.yml`: manage files, lines, downloads, and archives.
- `user_group_management.yml`: create or remove users and groups.
- `epel_repository.yml`: enable and inspect the EPEL repository on Red Hat family hosts.

#### `playbook/web_install/`

This scenario installs a basic Apache stack:

- adds `epel-release`
- installs SELinux Python bindings
- checks SELinux status
- installs `httpd`
- starts and enables the Apache service

Run it with:

```bash
ansible-playbook -i hosts site.yml
```

### `deepdive/`

These directories are mini exercises for common operations.

#### `deepdive/Playbook1/`

`bootstrap.yml` covers a multi-host bootstrap workflow:

- updates `/etc/hosts`
- installs packages such as `elinks` and `nmap-ncat`
- creates users for audit and network roles
- copies local files to managed hosts
- places an archive on a storage mount for sysadmin systems

#### `deepdive/playbook2/`

This set focuses on core playbook building blocks:

- `vars.yml`: variables
- `facts.yml`: fact-driven tasks
- `handler.yml`: handlers
- `loop.yml` and `loop2.yml`: loop examples
- `template/config.j2` and `template/template.yml`: template rendering

### `Playbook_Syntax/`

These files isolate advanced execution and control-flow features:

- `async.yml`: background task execution with `async` and `poll`.
- `blck_rescue_always.yml`: block, rescue, and always patterns.
- `delegate.yml`: delegated execution and local actions.
- `ignore_error.yml`: tolerating or reclassifying failures.
- `run_once.yml`: single-run task behavior.
- `tags.yml`: selecting or skipping tagged tasks.
- `vault.yml`: storing or consuming encrypted values.

Common tag commands used with this area:

```bash
ansible-playbook tags.yml --tags software
ansible-playbook tags.yml --skip-tags software
ansible-playbook tags.yml -t software,files
```

## How To Use This Repository

### 1. Prepare the controller

Use the setup notes in `install_config` to:

- install Ansible and Git
- create an `ansible` user
- configure passwordless SSH
- grant sudo access where needed
- define your inventory

### 2. Validate connectivity

Typical checks from the notes:

```bash
ansible localhost -m ping
ansible -i inv centos -m ping
ansible -i inv remote -m setup -a "filter=*dist*"
```

### 3. Practice in layers

Recommended progression:

1. Start with the top-level note files for ad-hoc command usage.
2. Compare them with `playbook/ad_hoc_to_playbook/` to see the playbook equivalent of common commands.
3. Move to `playbook/sample/` for individual playbook concepts.
4. Use `deepdive/` for scenario-style exercises.
5. Finish with `Playbook_Syntax/` for execution control and advanced syntax.

## Notes and Caveats

- Several examples use older module style syntax that is still useful for learning, but modern playbooks often prefer fully qualified modules and newer package abstractions.
- Host group names such as `centos`, `remote`, `webserver`, `qa-servers`, and `dbsystems` must exist in your inventory before running the examples unchanged.
- Some files are study notes rather than polished production playbooks. Review indentation, module names, and target hosts before using them in a real environment.
- Commands in the plain-text note files are intended as quick lab references and may need adjustment for your distribution or Ansible version.

## Suggested Study Path

If you are using this repo as a learning notebook, this order works well:

1. `install_config`
2. `static_inventory`
3. `module_shell`
4. `soft_install`, `daemons_services`, `user_groups`, and `file_manipulation`
5. `playbook/ad_hoc_to_playbook/`
6. `facts.yml`
7. `playbook/sample/`
8. `deepdive/`
9. `Playbook_Syntax/`
