# Playbook Samples

This directory is a concept-by-concept study area for writing Ansible playbooks.

## Topics Covered

- `play.yml` and `sample.yml`: basic playbook structure and task layout.
- `vars.yml` and `vars_play.yml`: variables, lists, and dictionaries.
- `facts.yml`: collecting and using facts.
- `cond.yml`: conditional execution.
- `loop.yml` and `loop2.yml`: iterating over lists and task inputs.
- `handler.yml`: handlers and change-driven service actions.
- `delegate.yml`: delegated execution and controller-side actions.
- `async.yml`: asynchronous work.
- `run-once.yml`: tasks that should execute once per play.
- `serial.yml`: rolling updates.
- `tags.yml`: selective execution with tags.
- `error1.yml` and `error2.yml`: error-handling behavior.
- `secret.yml`: handling sensitive values.
- `template.yml`, `config.j2`, and `httpd.conf.j2`: Jinja2 templates.
- `conclusion.yml`: wrap-up example file.

## Suggested Order

1. `play.yml`
2. `sample.yml`
3. `vars.yml` and `vars_play.yml`
4. `facts.yml`
5. `cond.yml`
6. `loop.yml` and `loop2.yml`
7. `handler.yml`
8. `template.yml`
9. `delegate.yml`, `async.yml`, `run-once.yml`, and `serial.yml`
10. `tags.yml`, `error1.yml`, `error2.yml`, and `secret.yml`

## Usage

Run a sample with your own inventory file or target localhost where appropriate:

```bash
ansible-playbook -i <inventory> play.yml
```

Use these files as building blocks. They are most useful when you compare one example to another and observe how the structure changes with each feature.
