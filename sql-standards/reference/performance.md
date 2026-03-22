# SQL Performance Reference

## EXPLAIN usage

```sql
EXPLAIN SELECT u.name, COUNT(o.id) as order_count
FROM users u
LEFT JOIN orders o ON u.id = o.user_id
WHERE u.is_active = 1
GROUP BY u.id;
```

Look for:
- `type: ALL` → full table scan, needs index
- `rows` → high number = slow
- `Extra: Using filesort` → add ORDER BY index

## Index strategies

```sql
-- Single column (most common)
CREATE INDEX idx_users_email ON users(email);

-- Composite (order matters: most selective first)
CREATE INDEX idx_orders_user_status ON orders(user_id, status);

-- Covering index (query only needs indexed columns)
CREATE INDEX idx_users_active_name ON users(is_active, name);
```

## Pagination (efficient)

```sql
-- AVOID for large offsets (slow)
SELECT * FROM orders LIMIT 100 OFFSET 10000;

-- GOOD — cursor-based pagination
SELECT * FROM orders WHERE id > :last_id ORDER BY id ASC LIMIT 100;
```

## Batch operations

```sql
-- Insert multiple rows in one query
INSERT INTO log_entries (user_id, action, created_at) VALUES
  (1, 'login', NOW()),
  (2, 'view', NOW()),
  (3, 'logout', NOW());

-- Update multiple rows efficiently
UPDATE items SET is_active = 0 WHERE id IN (1, 2, 3, 4, 5);
```
