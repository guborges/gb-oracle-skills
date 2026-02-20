# SQL Writing Standards for Production Oracle

## Context
Poor SQL shape causes avoidable CPU saturation, latch pressure, undo/redo inflation, and lock chains. In high-throughput CDB/RAC systems, marginal SQL mistakes become cluster-wide incidents.

## Mandatory Validation Checklist
- Verify object ownership, container, and edition context before writing SQL.
- Confirm required indexes and constraints exist and are valid.
- Check table and index stats freshness, histogram relevance, and stale objects.
- Verify expected row counts/cardinality by predicate before writing DML.
- Validate lock tolerance and concurrency impact for target objects.

## Recommended Decision Process
1. Define exact business predicate and cardinality target before composing SQL.
2. Start with projection minimization and predicate specificity; avoid broad scans by default.
3. Validate execution plan realism using current stats and bind behavior.
4. For DML, design chunking, commit cadence, and restartability first.
5. Add hints only if root cause analysis proves optimizer misestimation and safer fixes are not viable.

## Technical Notes
- Bind variables reduce hard parse pressure and shared pool churn; monitor bind peeking and skew.
- `SELECT *` increases I/O and network payload, and can defeat index-only access opportunities.
- Unbounded DML spikes redo/undo, extends checkpoint pressure, and raises log switch frequency.
- Function-wrapped predicates on indexed columns disable efficient access unless function-based indexes exist.
- Correlated subqueries and implicit datatype conversions often create hidden full scans and latch stress.

## Common Anti-Patterns
- Building SQL from guessed cardinality without validation queries.
- Relying on hints as first response instead of fixing stats, indexing, or SQL shape.
- Executing large updates/deletes in one transaction without restart markers.
- Ignoring bind sensitivity for skewed predicates and then blaming random plan changes.

## Production Risk & Mitigation
Risk: blocking storms, prolonged recovery due to redo growth, and unstable response times. Mitigation: preflight plan checks, bounded DML batches, explicit predicates, bind-aware testing, and rollback checkpoints per batch.
