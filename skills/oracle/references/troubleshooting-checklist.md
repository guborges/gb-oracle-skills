# Oracle Production Troubleshooting Checklist

## Context
During incidents, uncontrolled investigation increases downtime. A deterministic triage path is required to isolate fault domains quickly across SQL, instance, cluster, storage, and replication layers.

## Mandatory Validation Checklist
- Confirm incident scope: affected services, PDBs, nodes, and business transactions.
- Capture current state snapshots before changes (sessions, waits, blockers, lag, storage).
- Identify first failure timestamp and correlate with deployment, failover, or maintenance events.
- Verify alert log and critical trace files for ORA errors tied to timeline.
- Classify issue type: availability, performance, consistency, or recoverability.

## Recommended Decision Process
1. Stabilize first: stop blast-radius expansion (throttle jobs, drain problematic services, pause risky automation).
2. Build timeline from evidence, not assumptions.
3. Narrow by layer: SQL/optimizer, locking/concurrency, memory/I/O, RAC interconnect, Data Guard transport/apply.
4. Test minimally invasive hypotheses using read-only diagnostics.
5. Apply smallest reversible corrective action and re-measure against incident baseline.

## Technical Notes
- Blocking chains often hide root cause; inspect blocker workload and transaction age, not only victims.
- Wait-class shifts after a change can indicate secondary bottlenecks created by the first “fix.”
- Redo/archivelog anomalies may reveal hidden batch spikes or replication distress.
- Latch/mutex pressure can be parse-related and misdiagnosed as pure CPU shortage.
- In multitenant systems, incident symptoms may localize to one PDB while shared resources are CDB-level.

## Common Anti-Patterns
- Restarting instances early and destroying volatile diagnostic evidence.
- Jumping to parameter changes before isolating top offending SQL/workload.
- Treating RAC node differences as random noise rather than placement/contention signals.
- Declaring recovery after symptom drop without verifying lag, error recurrence, and workload integrity.

## RAC / Data Guard Impact
Incident triage must include node-level variance, service placement, transport/apply health, and role-transition history; otherwise corrective actions may worsen failover readiness.

## Production Risk & Mitigation
Risk: prolonged outage from misdiagnosis, recurrence due to shallow fixes, and DR posture degradation during firefight. Mitigation: evidence-first triage, preserved timelines, reversible interventions, and explicit verification of standby/cluster health before closure.
