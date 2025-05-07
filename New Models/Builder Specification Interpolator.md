---
date created: 2024-09-24 14:38
aliases:
    - BSI
    - model interpolator
---

The BSI is code that reads a given [[Builder Specification]] file, resolves all it's ancestry and dependency values, and produces an interpolated document with all possible values filled in.  If any values are left out, then the document is invalid and building cannot proceed.  The build process fails with errors.

The resulting YAML is then delivered to the [[BuildExecutor]] as the "buildspec".

