# fix(deps): force proxy stacks onto netmask 2.0.2

### Summary
- Pin every dependency path that pulls `netmask` through `pac-resolver`, `proxy-agent`, or `superagent-proxy` so they now install the patched 2.0.2 release and eliminate CVE-2021-28918.
- Regenerate the root and mbTest lockfiles so the archived dependency tree no longer references the vulnerable 1.0.6 tarball.

### Context
- Trivy flagged our builds because nested dependencies were still shipping `netmask` 1.0.6, which contains a CIDR parsing bug that attackers can use to bypass IP-based allowlists.

### Changes
- Added explicit overrides in `package.json` and `mbTest/package.json` so that each of the three proxy-related packages (`pac-resolver`, `proxy-agent`, and `superagent-proxy`) resolves `netmask` to 2.0.2; we need one override per package because each ships its own copy of `netmask` deep in the dependency tree.
- Re-ran `npm install` at the repo root and within `mbTest/` to refresh both `package-lock.json` files, ensuring the new overrides are locked in and the vulnerable artifact is removed.
- Did not touch any application source files or tests; the runtime behavior is unchanged outside of the security fix.

### Validation
- npm test *(not re-run in this iteration; change only adjusts dependency metadata and previously failed here due to container networking limits)*

### Impact & Risks
- Dependency change only: the major-version jump comes from the `netmask` maintainers, so proxy features that rely on IP parsing should get a quick smoke test in an environment that can reach real networks.
