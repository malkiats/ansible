In Ansible, secrets are sensitive data like passwords, API keys, SSH keys, and tokens. The correct way to handle them is Ansible Vault, not plain variables.

---

# 1. Simple (NOT safe - only for learning)

```yaml
vars:
  db_password: "MySecret123"
```

Problem:

- Stored in plain text
- Anyone can read it from Git
- Not safe for production

---

# 2. Recommended: Ansible Vault (BEST PRACTICE)

## Step 1: Create encrypted file

```bash
ansible-vault create secrets.yml
```

You will be prompted for a vault password.

---

## Step 2: Add secrets inside file

```yaml
db_user: admin
db_password: MySecret123
api_key: ABCD-XYZ-987
```

After saving, the file becomes encrypted like:

```text
$ANSIBLE_VAULT;1.1;AES256
3c9d8a8f9b1c....
```

---

## Step 3: Use secrets in playbook

```yaml
- name: Use vault secrets
  hosts: servers
  vars_files:
    - secrets.yml

  tasks:
    - name: Show DB user
      debug:
        msg: "User is {{ db_user }}"
```

---

## Step 4: Run playbook with vault password

```bash
ansible-playbook playbook.yml --ask-vault-pass
```

---

# 3. Vault with password file (CI/CD friendly)

## Create password file

```bash
echo "MyVaultPassword" > vault_pass.txt
chmod 600 vault_pass.txt
```

## Run playbook

```bash
ansible-playbook playbook.yml --vault-password-file vault_pass.txt
```

---

# 4. Encrypt only a variable (inline secret)

```bash
ansible-vault encrypt_string 'MySecret123' --name 'db_password'
```

Output:

```yaml
db_password: !vault |
          $ANSIBLE_VAULT;1.1;AES256
          6631323938...
```

Use directly in playbook:

```yaml
vars:
  db_password: !vault |
    $ANSIBLE_VAULT;1.1;AES256
    6631323938...
```

---

# 5. Example: Real DevOps usage (DB + API + SSH key)

```yaml
- name: Deploy app
  hosts: servers
  become: yes
  vars_files:
    - secrets.yml

  tasks:
    - name: Configure app config
      template:
        src: app.conf.j2
        dest: /etc/app.conf
```

Template (`app.conf.j2`):

```ini
DB_USER={{ db_user }}
DB_PASSWORD={{ db_password }}
API_KEY={{ api_key }}
```

---

# 6. Best practices (VERY IMPORTANT)

## DO

- Use Ansible Vault
- Keep secrets in `secrets.yml`
- Use CI/CD vault password injection
- Rotate secrets regularly

## DON'T

- Never store passwords in `group_vars/all.yml` in plain text
- Never commit secrets to GitHub or GitLab
- Never print secrets using debug

Bad example:

```yaml
- debug:
    msg: "{{ db_password }}"
```

---

# Real-world DevOps mapping

| Secret Type | Example |
|---|---|
| DB password | MySQL or Postgres password |
| API key | AWS, GitHub, or GitLab token |
| SSH private key | Server access |
| TLS cert | HTTPS certificates |
| Cloud credentials | AWS access key |
