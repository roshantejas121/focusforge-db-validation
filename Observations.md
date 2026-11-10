# FocusForge DB-Level Validation Observations

## Issues Found

The original schema created the three tables with only primary keys on the `id` columns. It did not protect required business data or relationships:

- `users.name` and `users.email` accepted `NULL` values.
- Duplicate email addresses were accepted, which could make account identity and login ambiguous.
- `projects.project_name` accepted `NULL` values.
- `tasks.title` accepted `NULL` values, making a task unidentifiable.
- `tasks.priority` accepted any integer, including values outside the application's 1–5 range.
- `tasks.project_id` was only an integer column and was not linked to `projects`, so orphan tasks could reference a project that does not exist.

## Constraints Implemented

- `NOT NULL` was added to `users.name`, `users.email`, `projects.project_name`, `tasks.title`, `tasks.priority`, and `tasks.project_id`. These values are required for valid records.
- `UNIQUE` was added to `users.email` so each email address identifies at most one user.
- A `CHECK (priority BETWEEN 1 AND 5)` constraint was added to `tasks.priority` to enforce the FocusForge priority domain.
- A foreign key from `tasks.project_id` to `projects.id` was added so every task must reference an existing project.

## Result

The valid records in `sample_data.sql` continue to insert successfully. After the constrained schema is loaded, the four invalid statements in `invalid_data.sql` are rejected by PostgreSQL:

1. A `NULL` task title is rejected by the `tasks.title` `NOT NULL` constraint.
2. The duplicate Alice email is rejected by the users email unique constraint.
3. Priority `10` is rejected by the priority check constraint.
4. Project ID `999` is rejected by the tasks-to-projects foreign key.

The database now enforces correctness for direct SQL writes, migration scripts, seed jobs, and application writes alike.
