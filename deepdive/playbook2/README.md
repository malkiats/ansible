# Deep Dive 2: Core Playbook Building Blocks

This directory groups together short examples for the features most frequently used in day-to-day playbooks.

## Files

- `vars.yml`: variable declaration and variable-driven tasks.
- `facts.yml`: collecting and using host facts.
- `handler.yml`: notifying handlers when a task changes something.
- `loop.yml` and `loop2.yml`: loop patterns.
- `template/template.yml`: template deployment example.
- `template/config.j2`: Jinja2 template used by the template example.

## Learning Focus

Use this directory to understand how core Ansible concepts interact:

- variables feed templates and conditions
- facts help playbooks react to the target host
- loops reduce duplicated task definitions
- handlers keep restarts or reloads change-driven

## Suggested Approach

1. Start with `vars.yml` and `facts.yml`.
2. Move to `loop.yml` and `loop2.yml`.
3. Finish with `handler.yml` and the files under `template/`.

These examples are intentionally small so the behavior of each feature is easy to isolate.
