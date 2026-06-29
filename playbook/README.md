# Playbook Directory

This directory collects general playbook examples and notes for common Ansible execution patterns.

## What You Will Find Here

- `linuxpatch.yml`: a rolling patching example that checks for running application processes, applies package updates, and handles reboot decisions.
- `playbook_sample_web`: a note file related to simple web deployment exercises.
- `task_in_play`: notes about placing tasks directly inside a play.
- `ad_hoc_to_playbook/`: playbook equivalents for several root-level ad-hoc note files.
- `sample/`: focused playbook examples for variables, loops, handlers, templates, tags, async execution, and more.
- `web_install/`: a small Apache installation scenario using a dedicated inventory file.

## Recommended Learning Order

1. Start with `ad_hoc_to_playbook/` if you want to compare one-off commands with repeatable playbooks.
2. Continue with `sample/` to learn one concept at a time.
3. Review `web_install/` for a simple multi-task service deployment workflow.
4. Read `linuxpatch.yml` as an example of operational automation with serial execution and guard rails.

## Notes

- Several files are lab examples rather than production-ready playbooks.
- Some examples use older syntax styles that remain useful for learning but may need modernization before real use.
- Validate target inventory groups before running any playbook unchanged.
