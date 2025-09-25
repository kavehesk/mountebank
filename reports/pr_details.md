# Pull Request Details

## Title
fix(mbTest): Override netmask to fix CVE-2021-28918

## Description
This change addresses the netmask vulnerability (CVE-2021-28918) in the mbTest directory by using an npm override to force the netmask version to 2.0.1. This is the recommended approach for resolving vulnerabilities in transitive dependencies when the direct dependency cannot be updated.

### Commit Message
fix(mbTest): Override netmask to fix CVE-2021-28918

The `w3cjs` package has a transitive dependency on `netmask@1.0.6`, which has a known vulnerability (CVE-2021-28918). This change uses an npm override to force the `netmask` version to `2.0.1`, which is a patched version.

This addresses the vulnerability without requiring an update to `w3cjs`, which is already at its latest version.