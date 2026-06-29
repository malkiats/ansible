# Collections

Install required collections before running playbooks:

```bash
ansible-galaxy collection install -r requirements.yml
```

Keep third-party collection requirements declared in one place so local development and CI pipelines use the same dependency set.
