# Performance Mindset for Oracle Incident and Tuning Work

## Context
Performance work becomes critical when latency breaches SLOs, throughput collapses, or RAC nodes show asymmetric stress. Fast but shallow fixes often convert acute incidents into chronic instability.

## Mandatory Validation Checklist
- Identify top waits and time model contributors (`v$system_event`, ASH/AWR scope).
- Isolate whether pressure is CPU, I/O, concurrency, commit, or interconnect (RAC GC).
- Validate current execution plans and row source stats for top SQL IDs.
- Confirm stats freshness and recent object/data distribution changes.
- Check memory posture: shared pool pressure, PGA over-allocation, and workarea spill indicators.

## Recommended Decision Process
1. Frame problem as DB time distribution, not single-metric intuition.
2. Separate symptom layer (wait class) from cause layer (SQL design, stats, schema, resource config).
3. Prioritize fixes by reversibility and blast radius: SQL/profile/baseline before structural changes.
4. Validate improvement under representative concurrency, not isolated single-session tests.
5. Require post-change guardrails: regression detectors, rollback criteria, and observation window.

## Technical Notes
- Optimizer cardinality errors propagate to join order, join method, memory grants, and temp spill behavior.
- Latch/mutex contention can be parse-driven; reduce hard parses before adding CPU.
- Excessive redo from write-heavy bursts can manifest as `log file sync` and downstream standby lag.
- In RAC, GC wait patterns (`gc cr/current`) can indicate object affinity issues and poor service placement.
- PGA/SGA imbalance can shift workload from memory operations to temp I/O and increase elapsed time variance.

## Common Anti-Patterns
- Tuning from AWR “top SQL” lists without validating business criticality and concurrency context.
- Applying system-wide parameter changes to fix one SQL regression.
- Treating temporary hint-based relief as permanent correction.
- Ignoring standby apply impact while optimizing primary commit throughput.

## RAC / Data Guard Impact
RAC tuning must account for instance affinity, service-level routing, and interconnect saturation. Data Guard requires checking whether primary-side tuning changes redo profile enough to increase apply lag or transport stress.

## Production Risk & Mitigation
Risk: regression transfer to different SQL, node imbalance, or DR objective degradation. Mitigation: phased rollout by service, SQL-level controls (baselines/profiles), and real-time lag/GC monitoring with predefined rollback triggers.
