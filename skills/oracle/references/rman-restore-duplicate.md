# RMAN Restore and Duplicate in Production Conditions

## Context
Restore/duplicate actions are time-critical during outages, corruption events, migrations, and refresh pipelines. Mistakes here directly extend downtime or produce unrecoverable inconsistencies.

## Mandatory Validation Checklist
- Validate backup completeness: datafiles, archivelogs, controlfile, SPFILE, and autobackup visibility.
- Confirm target recovery objective (SCN/time/log sequence) and business-approved data-loss tolerance.
- Verify source/target DBID, DB_NAME/DB_UNIQUE_NAME, and catalog/controlfile metadata consistency.
- Check ASM/FRA capacity and directory/OMF mapping for restore destination.
- Confirm channel parallelism, media manager access, and network throughput assumptions.

## Recommended Decision Process
1. Choose recovery endpoint first (complete vs incomplete/PITR) before command construction.
2. Decide controlfile strategy (current, restored, or backup controlfile) based on incident type.
3. Build deterministic runbook: restore, recover, validate, open/resetlogs implications.
4. For duplicate, isolate identity/network/service conflicts before instantiating target.
5. Validate restored state with structural and logical checks before handoff.

## Technical Notes
- `RESETLOGS` creates new incarnation; downstream backup and recovery chains must be re-baselined.
- Archivelog gap handling is often the critical path; pre-check sequence continuity and retrieval latency.
- Block media recovery is useful for bounded corruption but must be reconciled with standby consistency.
- In CDB/PDB, scope of restore differs (whole CDB vs PDB PITR); ensure dictionary/state alignment.
- RMAN duplicate with active database can saturate network/redo and affect primary workload if throttling is absent.

## Common Anti-Patterns
- Starting restore before confirming achievable recovery point.
- Ignoring DBID/incarnation details and attaching wrong catalog metadata.
- Performing duplicate without predefining filename conversion and ASM placement.
- Skipping post-recovery validation beyond “database opened successfully.”

## RAC / Data Guard Impact
RAC restores require instance and thread awareness for redo/undo handling. Data Guard environments require deciding whether recovery changes mandate standby rebuild, incremental roll-forward, or new baseline backups.

## Production Risk & Mitigation
Risk: failed recovery window, wrong-point recovery, and prolonged outage from iterative retries. Mitigation: prevalidated backup inventory, scripted checkpointed runbooks, explicit endpoint confirmation, and immediate post-restore backup/reset strategy.
