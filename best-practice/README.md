# Ansible Best Practices

Writing Ansible is not only about knowing the syntax. Syntax is a small part of the work. The larger part is building automation that is maintainable, secure, reusable, and trusted by multiple teams.

This guide turns that idea into a practical reference for structuring projects, writing cleaner playbooks, and operating Ansible safely in shared environments.

## The Core Idea

A strong Ansible project should be:

- easy to understand
- safe to run more than once
- simple to extend
- secure with secrets handled correctly
- reusable across teams and environments

A useful rule of thumb is this:

- syntax is about making Ansible run
- best practices are about making automation survive real operations

## 1. Organize Your Project Properly

Use a predictable project layout so engineers can find inventories, roles, variables, templates, and vault data without guessing.

```text
ansible-project/
├── ansible.cfg
├── inventory/
├── playbooks/
├── roles/
├── group_vars/
├── host_vars/
├── collections/
├── vault/
├── requirements.yml
└── README.md
```

Why this matters:

- reduces confusion
- makes onboarding faster
- helps CI pipelines and automation tools work consistently
- creates a structure that scales beyond one engineer

## 2. Keep Playbooks Small

A playbook should describe workflow and orchestration, not every low-level implementation detail.

Good playbooks usually answer questions like:

- which hosts are being targeted
- which roles are being applied
- what order the major steps run in

Keep heavy implementation inside roles.

## 3. Put Logic Inside Roles

A useful way to think about the relationship is:

- playbook = manager
- role = team
- task = individual engineer

A playbook coordinates work. Roles do the work.

Example:

```yaml
roles:
  - nginx
  - java
  - application
```

This keeps playbooks readable and lets the same role be reused by many different playbooks.

## 4. Make Roles Reusable

Reusable automation saves time and reduces inconsistencies.

A reusable role should:

- avoid environment-specific hardcoding
- accept variables for behavior
- contain a single clear purpose
- include defaults, handlers, templates, and documentation where needed

If a role only works for one exact server with one exact value set, it is not really reusable.

## 5. Never Hardcode Values

Use variables for anything that can change:

- package names
- ports
- file paths
- environment-specific URLs
- user names
- service names

Hardcoded values make automation brittle and harder to reuse.

## 6. Keep Secrets Out of Playbooks

Do not place secrets directly in plain-text playbooks, inventories, or templates.

Use Ansible Vault or another approved secrets manager for:

- passwords
- API keys
- certificates
- tokens
- sensitive configuration values

Benefits:

- better security
- safer collaboration
- easier version control without leaking secrets

## 7. Prefer Idempotent Tasks

A well-written playbook should reach the same desired state every time it runs.

That means:

- the first run makes the required changes
- later runs avoid unnecessary changes
- repeated runs stay predictable

Idempotency is one of the most important qualities in trusted automation.

## 8. Use Handlers

Do not restart services after every run unless configuration actually changed.

Use handlers so service restarts happen only when required.

Why this matters:

- avoids unnecessary downtime
- reduces operational noise
- keeps runs clean and intentional

## 9. Use Meaningful Names

Task names, role names, variable names, and inventory group names should describe intent clearly.

Prefer names like:

- Install Apache package
- Render application configuration
- Restart nginx when config changes

Avoid vague names like:

- task 1
- configure stuff
- run command

Meaningful names make troubleshooting and reviews much easier.

## 10. Prefer Modules Over Shell Commands

Use Ansible modules whenever possible.

Modules are usually:

- idempotent
- safer
- easier to understand
- more portable

Use `shell` or `command` only when no suitable module exists or when the task genuinely requires command execution.

## 11. Use Templates Instead of Multiple Config Files

Use Jinja2 templates with variables instead of maintaining separate copies of nearly identical configuration files.

This allows one template to support many environments with different values.

Benefits:

- less duplication
- easier updates
- cleaner environment-specific customization

## 12. Separate Environments

Do not edit playbooks to switch between development, test, staging, and production.

Use separate inventories or inventory directories instead.

Example:

```bash
ansible-playbook -i inventory/prod.ini deploy.yml
```

This reduces risk and makes deployments more consistent.

## 13. Use Group Variables

If all web servers share the same configuration, define it once in `group_vars/` instead of repeating it across multiple places.

This gives you:

- less duplication
- simpler maintenance
- better consistency across similar hosts

## 14. Keep Roles Focused

Each role should have one clear responsibility.

Examples of focused roles:

- `nginx`
- `java`
- `users`
- `application`
- `monitoring-agent`

Avoid large mixed-purpose roles that install packages, configure databases, deploy apps, and manage users all in one place.

## 15. Validate Before Production

A safe delivery flow usually looks like this:

```text
Syntax Check
    ↓
Linting
    ↓
Test Environment
    ↓
Staging
    ↓
Production
```

Automate these checks with CI or CD pipelines whenever possible.

Typical validation steps include:

- syntax validation
- ansible-lint
- dependency installation
- test execution
- deployment to lower environments before production

## 16. Version Control Everything

Keep the following in Git:

- playbooks
- roles
- templates
- inventories without secrets
- documentation

Treat automation the same way you treat application code.

Why this matters:

- traceability
- peer review
- rollback history
- collaboration across teams

## 17. Keep Inventories Clean

Use logical groups and consistent naming.

A clean inventory makes targeting easy and reduces mistakes during deployments.

Examples:

- group by environment
- group by application tier
- group by function such as `web`, `db`, or `monitoring`

## 18. Test Roles Independently

Do not wait for a full platform deployment to learn whether one role works.

Each role should be testable on its own. That shortens feedback loops and makes debugging easier.

This is especially important when a role is reused across many playbooks.

## 19. Make Automation Easy to Understand

Assume another engineer joins six months later and needs to work with your automation quickly.

They should be able to answer:

- What does this playbook do?
- Why does this variable exist?
- Which environments use this role?

Add a README to the project and to important roles. Good documentation reduces onboarding time and prevents mistakes.

## 20. Think Like a Platform Engineer

Ask a better question than "Does this run?"

Ask:

- Can every engineer use this safely?
- Can another team reuse it without rewriting it?
- Can it pass through automation and review confidently?
- Would I trust it to run automatically in production at 2 AM?

That is the difference between a script and an operational platform capability.

## Delivery Model

A mature automation flow often looks like this:

```text
Developer updates playbook
            │
            ▼
Push to Git
            │
            ▼
CI Pipeline
├── ansible-lint
├── syntax check
├── install collections
└── run automated tests
            │
            ▼
Deploy to development
            │
            ▼
Validate
            │
            ▼
Promote to staging
            │
            ▼
Production deployment
            │
            ▼
Generate reports and notify
```

## Best Practices Checklist

| Area | Best Practice | Why It Matters |
| --- | --- | --- |
| Structure | Use a standard project layout | Easier to navigate and maintain |
| Playbooks | Keep them short and focused | Better readability |
| Roles | Make them reusable | Avoid duplication |
| Variables | Never hardcode values | More flexibility |
| Secrets | Use Vault or a secrets manager | Better security |
| Tasks | Prefer idempotent modules | Safe repeatability |
| Handlers | Restart only when needed | Less disruption |
| Templates | Use Jinja2 for configs | One template for many environments |
| Inventory | Separate dev, test, and prod | Safer deployments |
| Validation | Lint, syntax check, and test before production | Catch issues early |
| Version Control | Store automation in Git | Traceability and collaboration |
| Documentation | Include READMEs and clear task names | Easier onboarding and maintenance |

## Leadership Perspective

Use these questions as a final quality filter:

- Can another engineer understand this in 10 minutes?
- Can another team reuse this without modification?
- Is it safe to run repeatedly?
- Does it protect secrets properly?
- Would I trust this to run automatically in production at 2 AM?

If the answer to any of these is no, the automation still needs work.

## Included Reference Material In This Repository

This repository now includes practical assets alongside the guidance in this document:

- `sample-production-ansible/`: a sample production-grade directory scaffold with separated inventories, scoped variables, playbooks, and role skeletons.
- `templates/role-readme-template-generic.md`: a general README template for infrastructure or platform roles.
- `templates/role-readme-template-application.md`: a role README template tuned for application deployment roles.

Use these as starting points, not fixed standards. Adapt them to your platform, environment model, and team conventions.
