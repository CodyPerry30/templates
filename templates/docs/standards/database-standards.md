# Database Standards

> **Scope**: SQL, migrations, schema design

## Naming

| Object | Convention | Example |
|--------|------------|---------|
| Tables | snake_case | `user_profile`, `order_item` |
| Columns | snake_case | `created_at`, `user_id` |
| Primary keys | `id` | `id BIGINT PRIMARY KEY` |
| Foreign keys | `<table>_id` | `user_id`, `order_id` |
| Indexes | `ix_<table>_<cols>` | `ix_users_email` |
| Constraints | `<type>_<table>_<desc>` | `pk_users`, `fk_orders_users` |

## Schema Design

```sql
-- ✅ Standard table structure
CREATE TABLE user (
    id BIGINT GENERATED ALWAYS AS IDENTITY PRIMARY KEY,
    email VARCHAR(255) NOT NULL,
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL,
    updated_at TIMESTAMP WITH TIME ZONE DEFAULT NOW() NOT NULL,
    deleted_at TIMESTAMP WITH TIME ZONE  -- soft delete
);

-- ✅ Always index foreign keys
CREATE INDEX ix_orders_user_id ON orders (user_id);

-- ✅ Naming constraints explicitly
CONSTRAINT fk_orders_users FOREIGN KEY (user_id) REFERENCES users(id)
```

## SQL Style

- UPPERCASE for keywords (`SELECT`, `FROM`, `WHERE`)
- Use explicit `AS` for aliases
- Use explicit `JOIN` syntax, not comma joins
- Always specify columns, avoid `SELECT *`

## Migrations

```
V1.0.0__create_users_table.sql
V1.0.1__add_email_index.sql
V1.1.0__create_orders_table.sql
```

- Add columns as nullable first, then add constraints
- Never drop columns directly—deprecate first
- Always provide rollback scripts

## Boundaries

- ✅ Always: Include `created_at`/`updated_at`, index foreign keys
- ⚠️ Ask first: Schema changes, adding tables, changing column types
- 🚫 Never: Use `SELECT *` in application code, store passwords in plain text

## Notes

- Prefer `BIGINT` or `UUID` for primary keys
- Use `TIMESTAMP WITH TIME ZONE` for dates
- Soft deletes with `deleted_at` preferred over hard deletes
