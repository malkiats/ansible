# Advanced Playbook Syntax

Advanced Inventory Configuration

Error Handling in a Playbook - limit, ignore_errors, changed_when, and failed_when

Delegating Playbook Execution with `delegate_to` and `local_action`

Working with Sensitive Data Using Ansible Vault

# Run Tag
ansible-playbook tags.yml --tags software 

ansible-playbook tags.yml --skip-tags software

ansible-playbook -t software,files
