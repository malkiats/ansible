# Ad-Hoc To Playbook Examples

This directory converts several root-level ad-hoc command notes into playbook form so the repository shows both styles side by side.

## Why This Exists

Ad-hoc commands are useful for:

- quick validation
- one-time changes
- learning modules interactively

Playbooks are better for:

- repeatable automation
- version-controlled changes
- clearer review and collaboration
- combining multiple related tasks into one workflow

## Included Playbooks

- `control_node_bootstrap.yml`: a localhost-oriented example derived from the controller setup notes.
- `module_shell_examples.yml`: connectivity, facts, package, command, and shell examples.
- `package_management.yml`: package install and removal examples.
- `service_management.yml`: service start, stop, enable, and disable examples.
- `file_management.yml`: file, copy, line editing, download, and archive tasks.
- `user_group_management.yml`: user and group examples.
- `epel_repository.yml`: enable and verify the EPEL repository.
-
## Notes

- These playbooks are examples, not a complete production role design.
- Some variables intentionally expose values you should customize before use.
- `install_config` and `static_inventory` remain reference notes because they describe controller preparation and inventory structure rather than only remote task execution.
