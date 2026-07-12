# Errors

## [ERR-20260712-003] Markdown trailing whitespace

**Logged**: 2026-07-12T14:04:00+08:00
**Priority**: low
**Status**: resolved
**Area**: docs

### Summary

The README initially used trailing spaces for a forced line break, which failed the Git whitespace check.

### Error

```text
README.md:5: trailing whitespace.
```

### Context

- Validating the paper-backed README with `git diff --check`.

### Suggested Fix

Use an explicit `<br>` element for intentional line breaks in centered HTML blocks.

### Metadata

- Reproducible: yes
- Related Files: README.md

---

## [ERR-20260712-002] PDF text extraction dependency

**Logged**: 2026-07-12T13:54:00+08:00
**Priority**: low
**Status**: resolved
**Area**: docs

### Summary

The system shell did not include Poppler's `pdftotext` utility while preparing paper-backed documentation.

### Error

```text
zsh: command not found: pdftotext
```

### Context

- Extracting content from the accepted SMDP manuscript for a README revision.
- The bundled workspace Python runtime included `pypdf` and `pdfplumber`.

### Suggested Fix

Use the bundled `pypdf` package for text extraction and macOS Quick Look for page rendering when Poppler is unavailable.

### Metadata

- Reproducible: yes
- Related Files: README.md

---

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
