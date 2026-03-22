---
name: sql-standards
description: Writes, reviews, and optimizes SQL queries following Quanture security and performance standards. Use when writing SQL queries, designing database schemas, creating migrations, optimizing slow queries, or reviewing database code.
---

# SQL Standards — Quanture

## Database: MySQL 8.0 (production)

Connection: `192.168.254.6` — database `clawbot`

## Security — non-negotiable

**Always use parameterized queries. No exceptions.**

```python
# Python — GOOD
cursor.execute("SELECT * FROM users WHERE id = %s AND active = %s", (user_id, True))

# Python — NEVER (SQL injection)
cursor.execute(f"SELECT * FROM users WHERE id = {user_id}")
```

```java
// Java — GOOD (PreparedStatement or JPA)
PreparedStatement ps = conn.prepareStatement("SELECT * FROM users WHERE id = ?");
ps.setInt(1, userId);

// Java — NEVER
stmt.execute("SELECT * FROM users WHERE id = " + userId);
```

```javascript
// Node.js — GOOD
db.query("SELECT * FROM users WHERE id = ?", [userId])

// NEVER
db.query(`SELECT * FROM users WHERE id = ${userId}`)
```

## Naming conventions

```sql
-- Tables: lowercase_snake_case, plural
CREATE TABLE user_accounts (...);
CREATE TABLE order_items (...);

-- Columns: lowercase_snake_case
created_at, updated_at, is_active, user_id

-- Indexes: idx_table_column
idx_users_email, idx_orders_user_id_created_at

-- Foreign keys: fk_table_column
fk_orders_user_id
```

## Table design template

```sql
CREATE TABLE items (
    id          BIGINT UNSIGNED AUTO_INCREMENT PRIMARY KEY,
    name        VARCHAR(255)    NOT NULL,
    description TEXT,
    is_active   TINYINT(1)      NOT NULL DEFAULT 1,
    created_at  DATETIME        NOT NULL DEFAULT CURRENT_TIMESTAMP,
    updated_at  DATETIME        NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    
    INDEX idx_items_is_active (is_active),
    INDEX idx_items_created_at (created_at)
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

## Migrations

- Use sequential numbered files: `001_create_users.sql`, `002_add_email_index.sql`
- Migrations are **forward-only** — no rollback in production without explicit approval
- Always test migrations on a copy of prod data first
- Include both UP and DOWN scripts in development

```sql
-- UP
ALTER TABLE users ADD COLUMN last_login DATETIME;

-- DOWN (for dev rollback only)
ALTER TABLE users DROP COLUMN last_login;
```

## Query performance rules

- Always add `LIMIT` to queries that could return many rows
- Use `EXPLAIN` before deploying slow queries
- Index foreign keys and columns used in `WHERE`, `JOIN`, `ORDER BY`
- Avoid `SELECT *` — list explicit columns

```sql
-- GOOD
SELECT id, name, email FROM users WHERE is_active = 1 LIMIT 100;

-- AVOID
SELECT * FROM users;
```

## Sensitive data

- Passwords: **never store plaintext** — store bcrypt hash
- Tokens/keys: store hashed or encrypted, never plaintext
- PII (emails, phones): consider encryption at rest for regulated data
- Never log query results that contain passwords or tokens

## Reference

- **Indexes & performance**: See [reference/performance.md](reference/performance.md)
- **Common queries**: See [reference/queries.md](reference/queries.md)
