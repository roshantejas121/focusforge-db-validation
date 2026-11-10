# FocusForge DB-Level Validation

This submission fixes the FocusForge schema with database-enforced validation and proves that invalid direct SQL writes are rejected by PostgreSQL.

## Contents

- `schema/schema.sql` — fixed schema with `NOT NULL`, `UNIQUE`, `CHECK`, and `FOREIGN KEY` constraints.
- `schema/sample_data.sql` — unchanged valid records; inserts successfully.
- `schema/invalid_data.sql` — four invalid inserts; all are rejected after the fix.
- `Observations.md` — issues found, constraints added, and results.
- `screenshots/` — one readable PostgreSQL rejection screenshot per test plus a summary PNG.
