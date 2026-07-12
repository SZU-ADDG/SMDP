# Errors

## [ERR-20260712-001] zsh reserved variable name

**Logged**: 2026-07-12T00:00:00+08:00
**Priority**: low
**Status**: resolved
**Area**: infra

### Summary

A read-only repository check stopped after assigning to zsh's reserved `status` parameter.

### Error

```text
zsh:3: read-only variable: status
```

### Context

- A shell command attempted to store an exit code in a variable named `status`.
- Repository inspection before GitHub publication.

### Suggested Fix

Use a neutral variable name such as `exit_code` in zsh scripts.

### Metadata

- Reproducible: yes
- Related Files: none

---
