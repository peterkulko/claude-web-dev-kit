---
name: db-schema-review
description: Reviews database schema definitions or migrations (SQL DDL, Prisma/Drizzle/TypeORM schema files, migration scripts) for correctness, normalization issues, missing constraints/indexes, and risky migration patterns. Use when the user asks for a schema/migration review or "is this migration safe".
---

# Database Schema Review

Audit schema definitions and migrations for correctness and safety before they're applied.

## Steps

1. Identify scope: the specific schema file(s) or migration(s) named by the user, or the most recent migration if not specified.

2. Read the target files and check for:
   - **Constraints**: missing `NOT NULL` on columns that shouldn't be nullable, missing foreign key constraints, missing unique constraints on fields that should be unique.
   - **Indexes**: missing indexes on foreign keys and frequently-queried/filtered columns; redundant or duplicate indexes.
   - **Types**: inappropriate column types (e.g. storing money as `float`, storing timestamps without timezone when the app spans timezones, using `varchar` without a sensible length where truncation matters).
   - **Normalization**: obvious repeating groups or redundant data that should be a separate table, without over-engineering a schema that's intentionally denormalized for performance.
   - **Migration safety**: adding a `NOT NULL` column without a default on a table that already has rows; renaming/dropping columns still referenced by application code; long-running locks on large tables (e.g. adding an index without `CONCURRENTLY` on Postgres); irreversible migrations with no down/rollback path.

3. For each issue found, report file and line number, what's wrong, a concrete failure scenario (e.g. "this will fail on any existing row" or "this locks the table for the duration of the index build"), and a suggested fix.

4. Do not rewrite migrations or schema files unless the user asks — present findings first.

5. If nothing significant is found, say so briefly rather than inventing minor issues.
