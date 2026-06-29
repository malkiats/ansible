# application Role

## Purpose

Deploy one application service with explicit variables, templates, and restart logic.

## Required Variables

```yaml
application_name: sample-app
application_version: 1.0.0
application_artifact_url: https://repo.example.com/sample-app-1.0.0.tar.gz
```

## Optional Variables

```yaml
application_user: sampleapp
application_group: sampleapp
application_install_dir: /opt/sample-app
application_service_name: sample-app
```

## Notes

Keep secrets such as API tokens or database passwords outside plain-text role files. Inject them through Vault or your secret-management workflow.
