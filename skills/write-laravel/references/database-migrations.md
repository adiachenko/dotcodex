# Database Migrations

Define only the `up` method; omit `down`, including default scaffolding.

Migrations are for schema changes only. Simple data manipulations tied to those changes (adjusting data for new columns/formats) are acceptable. Never seed application data or run business logic in migrations — use dedicated one-off console commands.

## Foreign Key Delete Policy

Give every foreign key an explicit `nullOnDelete()`, `cascadeOnDelete()`, or `restrictOnDelete()` policy.

### Many-to-Many Relationships

Use `cascadeOnDelete()` for pivot-table foreign keys.

### Owned Children

Use `cascadeOnDelete()` when the child is inseparable from its parent and has no independent meaning.

### Optional References

Use `nullOnDelete()` for nullable descriptive, attribution, or non-owning references. This is the default for `*_by`, `assigned_to`, `owner_id`, `parent_id`, and `last_*_id` columns.

### Business-Critical or Historical Records

Use `restrictOnDelete()` or soft deletes for financial, audit, security, legal, or historical data.

### Shared or Polymorphic Resources

Never cascade deletion through a resource referenced by multiple parents.

### Tie-Breaker

Cascade owned data, null non-owning references, and restrict deletion only for a clear business reason.
