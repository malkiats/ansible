# 1. Variables (`vars`)

Variables let you store values and reuse them.

```yaml
---
- name: Variables Example
  hosts: servers
  vars:
    package_name: git
    username: devops

  tasks:
    - name: Show package
      debug:
        msg: "Installing {{ package_name }} for {{ username }}"
```

Output:

```text
Installing git for devops
```

---

# 2. debug

Used to print information.

```yaml
tasks:
  - name: Print message
    debug:
      msg: "Deployment Started"
```

Output

```text
ok: [gitlab] =>
  msg: Deployment Started
```

You can also print variables.

```yaml
- debug:
    var: package_name
```

Output

```text
package_name: git
```

---

# 3. register

Stores the output of a task so you can use it later.

```yaml
tasks:
  - name: Check uptime
    command: uptime
    register: uptime_result

  - name: Show uptime
    debug:
      var: uptime_result.stdout
```

Output

```text
14:33:20 up 3 days, 5 users, load average...
```

Think of `register` as creating a variable from a command's output.

---

# 4. loop

Repeats a task.

Instead of writing:

```yaml
- package:
    name: git

- package:
    name: curl

- package:
    name: vim
```

Do this:

```yaml
vars:
  packages:
    - git
    - curl
    - vim

tasks:
  - name: Install packages
    package:
      name: "{{ item }}"
      state: present
    loop: "{{ packages }}"
```

During execution:

```text
item = git
item = curl
item = vim
```

---

# 5. when

Runs a task only if a condition is true.

```yaml
tasks:
  - name: Install Apache on Ubuntu
    apt:
      name: apache2
      state: present
    when: ansible_os_family == "Debian"
```

Another example

```yaml
- name: Install httpd
  dnf:
    name: httpd
    state: present
  when: ansible_os_family == "RedHat"
```

Very useful for multi-OS environments.

---

# 6. handlers

Handlers run **only when notified**.

Example:

```yaml
tasks:
  - name: Copy nginx config
    copy:
      src: nginx.conf
      dest: /etc/nginx/nginx.conf
    notify:
      - Restart nginx

handlers:
  - name: Restart nginx
    service:
      name: nginx
      state: restarted
```

Flow

```text
Copy file
↓
Was file changed?
↓
YES
↓
Restart nginx
```

If the file didn't change, Ansible **won't restart the service**, making playbooks idempotent.

---

# 7. templates

Templates let you generate configuration files dynamically using Jinja2.

Template file: `nginx.conf.j2`

```jinja
server {
    listen {{ port }};
    server_name {{ hostname }};
}
```

Playbook

```yaml
vars:
  port: 80
  hostname: mysite.com

tasks:
  - name: Create nginx config
    template:
      src: nginx.conf.j2
      dest: /etc/nginx/nginx.conf
```

Generated file

```nginx
server {
    listen 80;
    server_name mysite.com;
}
```

Templates are heavily used for configs like Nginx, HAProxy, Prometheus, and Kubernetes manifests.

---

# 8. roles

Roles organize your Ansible project into reusable components.

Without roles:

```text
playbook.yml
1000 lines
```

With roles:

```text
project/
├── playbook.yml
├── roles/
│   ├── nginx/
│   │   ├── tasks/
│   │   ├── handlers/
│   │   ├── templates/
│   │   ├── vars/
│   │   └── files/
│   └── docker/
```

Example playbook

```yaml
- name: Configure Servers
  hosts: servers
  roles:
    - nginx
    - docker
```

Ansible automatically loads:

- `tasks/main.yml`
- `handlers/main.yml`
- `templates/`
- `files/`
- `vars/main.yml`

This keeps projects modular and reusable.

---

# Putting Everything Together

Suppose you want to install and configure Nginx.

```yaml
---
- name: Install and Configure Nginx
  hosts: servers
  become: yes
  vars:
    package_name: nginx
    server_name: devops.local

  tasks:
    - name: Install nginx
      package:
        name: "{{ package_name }}"
        state: present

    - name: Copy nginx configuration
      template:
        src: nginx.conf.j2
        dest: /etc/nginx/nginx.conf
      notify: Restart nginx

    - name: Check nginx version
      command: nginx -v
      register: nginx_version
      changed_when: false

    - name: Show nginx version
      debug:
        var: nginx_version.stderr

  handlers:
    - name: Restart nginx
      service:
        name: nginx
        state: restarted
```

This example demonstrates:

- `vars` -> Store package name and server name.
- `template` -> Create a configuration file.
- `notify` -> Trigger a handler only if the template changes.
- `handler` -> Restart Nginx when needed.
- `register` -> Capture the output of `nginx -v`.
- `debug` -> Display the captured version.
- `become` -> Execute tasks with elevated privileges.

---

## A simple way to remember these concepts

| Feature | Purpose | Real-world analogy |
|---|---|---|
| `vars` | Store reusable values | Labels on boxes |
| `debug` | Print information | `echo` or `print()` |
| `register` | Save task output | Assigning a variable in code |
| `loop` | Repeat a task | `for` loop |
| `when` | Conditional execution | `if` statement |
| `handlers` | Run only after a change | Restart a service only when configuration changes |
| `templates` | Generate dynamic files | Mail merge or document template |
| `roles` | Organize automation | A reusable software module or package |
