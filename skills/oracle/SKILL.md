---
name: oracle
description: Production-grade Oracle DBA reasoning framework for generating and validating SQL, DDL, and recovery actions in Oracle 19c/23ai/26ai environments with CDB/PDB, RAC, Data Guard, and ASM constraints. Use when planning tuning, restore/duplicate, topology-aware operations, or any change that can affect availability, consistency, or recovery objectives.
---

# Oracle Skill Operating Framework

## Deterministic Reasoning Sequence Before Any SQL
1. Classify the request: read-only diagnosis, DML, DDL, topology change, backup/recovery, or parameter change.
2. Identify blast radius: PDB-local, CDB-wide, instance-local, cluster-wide (RAC), or primary/standby coupled (Data Guard).
3. Identify version-sensitive behavior for 19c/23ai/26ai before proposing syntax or feature usage.
4. State required preconditions explicitly (object ownership, statistics freshness, lock tolerance, maintenance window, rollback path).
5. Generate SQL only after validation queries are defined.
6. Provide side effects and operational risk before executable change statements.

## Required Environment Validation (Execute First)
- Confirm version and patch level: `v$instance`, `dba_registry_sqlpatch`.
- Confirm CDB/PDB context: `v$database.cdb`, `sys_context('USERENV','CON_NAME')`, open mode of target PDB.
- Confirm topology: RAC (`gv$instance`), Data Guard role/open mode (`v$database`, `v$archive_dest_status`), ASM dependency for storage paths.
- Confirm workload timing and contention signals: active sessions, blocking chains, top wait events.
- Confirm recovery safety: recent backup state, archive log health, and available FRA/ASM diskgroup headroom.

## SQL Generation Guardrails
- Use bind variables for repeat or application-path SQL unless literal selectivity is deliberately required and justified.
- Never assume optimizer statistics are correct; verify object and system stats freshness before diagnosing plans.
- Avoid optimizer hints unless a measured regression exists, root cause is identified, and hint scope/expiry is documented.
- Never emit `SELECT *` for operational SQL intended for production execution.
- Never emit unbounded `DELETE`/`UPDATE` without explicit predicate strategy, chunking plan, and commit/rollback controls.
- Never assume single-instance execution semantics; consider RAC global cache and service placement.
- Explain DDL side effects before proposing execution: invalidation, lock requirements, redo growth, replication/standby implications.

## Routing Rules to References
- If tuning, cardinality, plan stability, or wait analysis is central, use `references/performance-mindset.md`.
- If restore, PITR, duplicate, cloning, or controlfile/spfile recovery is involved, use `references/rman-restore-duplicate.md`.
- If RAC, Data Guard, ASM placement, or cross-node behavior affects decisions, use `references/rac-dg-asm.md`.
- If behavior depends on release differences or SQL feature availability, use `references/version-awareness.md`.
- For SQL construction and review discipline, use `references/sql-writing-standards.md`.
- For operational execution controls and change safety, use `references/production-safety.md`.
- For incident triage and narrowing fault domains, use `references/troubleshooting-checklist.md`.

## Explicit Prohibitions
- No destructive DDL in generated runbooks without backup/restore verification steps and rollback posture.
- No direct production changes without first producing read-only validation SQL.
- No assumption that statistics, histograms, or adaptive features are currently sane.
- No hidden-parameter recommendations unless explicitly requested and risk-scoped.
