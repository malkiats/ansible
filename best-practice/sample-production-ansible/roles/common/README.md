# common Role

## Purpose

Apply baseline operating system settings that should exist on every managed host.

## Typical Responsibilities

- install shared packages
- configure timezone or baseline system settings
- create common directories
- apply organization-wide hardening prerequisites

## Variables

### Optional variables

```yaml
common_packages:
  - vim
  - curl

timezone_name: UTC
```

## Notes

Keep this role limited to truly common concerns. Do not place application-specific logic here.
