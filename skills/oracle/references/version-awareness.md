# Version Awareness for 19c, 23ai, and 26ai

## Context
Version drift drives avoidable outages when SQL, optimizer behavior, and management features differ across estates. This is critical during cross-version fleet operations, rolling upgrades, and scripts shared between legacy 19c and AI releases.

## Mandatory Validation Checklist
- Capture exact RDBMS release and RU level from `v$instance` and `dba_registry_sqlpatch`.
- Confirm `compatible` parameter at CDB and target PDB scope.
- Validate feature prerequisites (initialization parameters, option enablement, patch dependencies).
- Confirm whether SQL will run in mixed-version Data Guard or transient upgrade states.
- Verify client/tooling compatibility (SQL*Plus, JDBC, RMAN catalog version).

## Recommended Decision Process
1. Start from minimum common denominator when deploying SQL across multiple versions.
2. Separate syntax compatibility from runtime behavior compatibility (optimizer transformations, stats usage, adaptive controls).
3. Determine whether change must be version-conditional; if yes, branch logic explicitly.
4. Prefer reversible operational sequencing: deploy read-only diagnostics first, then bounded changes.
5. Record downgrade/rollback feasibility before enabling version-gated features.

## Technical Notes
- `optimizer_features_enable` can mask behavior but does not fully recreate older execution environments.
- Stats model differences (histograms, directives, adaptive feedback lifecycle) can alter plans between RUs.
- SQL macros, JSON behavior, and AI-era SQL features may parse differently or be unavailable on 19c.
- Dictionary and component patch skew can produce plan instability and invalid objects after deployment.
- In CDB/PDB, patch and parameter posture can diverge by container; validate at PDB scope, not only root.

## Common Anti-Patterns
- Treating RU numbers as irrelevant “minor” updates.
- Shipping one SQL baseline or hint set to all versions without regression testing.
- Using 23ai/26ai syntax in automation that still targets 19c fallback paths.
- Ignoring `compatible` and assuming binary version alone controls behavior.

## RAC / Data Guard Impact
Mixed-version windows in RAC rolling maintenance and Data Guard switchover periods amplify incompatibility risk. Plan service routing and role transitions so version-dependent SQL is not executed on unexpected nodes/roles.

## Production Risk & Mitigation
Risk: parse failures, silent plan drift, failover-time regressions, and inconsistent behavior across nodes. Mitigation: enforce version-gated runbooks, validate on replica-like environments, keep feature flags reversible, and document fallback SQL paths.
