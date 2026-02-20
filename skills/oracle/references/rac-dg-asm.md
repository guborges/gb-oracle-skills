# RAC, Data Guard, and ASM Operational Reasoning

## Context
Topology-aware decisions are critical during failover, storage pressure, rolling maintenance, and any change that can cross node or site boundaries. Single-instance assumptions are a primary cause of clustered outages.

## Mandatory Validation Checklist
- Map current RAC services, preferred/available instances, and session distribution.
- Validate Data Guard role, transport mode, apply status, and lag thresholds.
- Verify ASM diskgroup redundancy, usable capacity, rebalance activity, and I/O hot spots.
- Confirm SPFILE/PFILE and clusterware-managed resource consistency across nodes.
- Check network dependencies: interconnect health, listener/service registration, and SCAN behavior.

## Recommended Decision Process
1. Identify control plane affected: clusterware, database role management, or storage layer.
2. Evaluate whether operation is node-local, cluster-wide, site-wide, or storage-global.
3. Sequence actions to preserve quorum, service availability, and recovery path at each step.
4. Execute least-disruptive path first (service relocate, drain, switchover rehearsal, then change).
5. Validate convergence criteria before proceeding to next stage.

## Technical Notes
- RAC cache fusion overhead rises when hot blocks bounce across instances; service affinity reduces GC traffic.
- Data Guard protection mode dictates tolerance to transport/apply disruption during maintenance.
- ASM rebalance competes for I/O bandwidth; unmanaged power settings can destabilize latency-sensitive workloads.
- Archivelog generation spikes from maintenance DDL/DML can overload transport or FRA if not pre-sized.
- CDB/PDB role transitions require validating service mapping and open state behavior per PDB.

## Common Anti-Patterns
- Running topology changes without service drain and session relocation.
- Treating standby lag as acceptable background noise during high-redo operations.
- Triggering ASM rebalance during known peak load without throttle strategy.
- Assuming node-level validation implies cluster-wide safety.

## RAC / Data Guard Impact
This topic is inherently RAC/Data Guard scoped: every change should include node impact, role-transition implications, and cross-site recovery posture.

## Production Risk & Mitigation
Risk: split-brain exposure, unplanned service brownouts, standby divergence, and storage-induced latency collapse. Mitigation: explicit stage gates, capacity headroom checks, controlled rebalance/transport parameters, and rehearsed failback steps.
