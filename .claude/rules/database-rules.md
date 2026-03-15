# Database Rules

## Naming
- Tables: `snake_case`, plural (`users`, `order_items`)
- Columns: `snake_case` (`created_at`, `first_name`)
- Primary keys: `id` (bigint/UUID)
- Foreign keys: `[referenced_table_singular]_id` (`user_id`, `order_id`)
- Indexes: `idx_[table]_[columns]` (`idx_users_email`)

## Migrations
- Use Flyway or Liquibase — never manual DDL in production
- Migration files: `V[number]__[description].sql` (Flyway)
- Every migration must be reversible or documented if not
- Never modify existing migrations — always add new ones

## JPA / Hibernate
- Use `@Entity` with explicit `@Table(name = "...")` 
- Map columns explicitly with `@Column(name = "...")`
- Use `FetchType.LAZY` by default — load eagerly only via `JOIN FETCH` in queries
- Avoid `CascadeType.ALL` — be explicit about cascades
- Use `@Version` for optimistic locking where needed

## Queries
- Use Spring Data JPA repository methods for simple queries
- Use `@Query` with JPQL for complex queries
- Use native queries only when JPQL is insufficient
- Always use parameterized queries — never concatenate values

## Performance
- Add indexes for frequently queried columns and foreign keys
- Use pagination (`Pageable`) for list endpoints
- Monitor and log slow queries in development
