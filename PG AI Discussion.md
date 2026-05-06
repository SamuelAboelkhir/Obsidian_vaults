---
tags: 
- Other
MOC: Programming
---

[[_0000 Home|Home]] | [[_0006 Programming MOC|Back to Programming MOC]] | [[PG Other index|Back to index]]

```
what column types are supported in db2? int32? int64? bigint? enums? I need to know for my data types

export const activitiesTable = sqliteTable('activities', {
  id: text().primaryKey().notNull(),
  activityDutyType: int().$type<DutyType>(),
  activityCodeName: text(),
  activityCode: int(),
  activityId: int(),
  addressId: text().references(() => addressesTable.id),
  date: text(),
  driverNo: int().references(() => driversTable.id),
  vehicleNo: text().references(() => vehiclesTable.id),
  odometerKm: real(),
  odometerMi: real(),
  context: int().$type<ActivityContext>(),
  proximityAddress: text(),
  changeState: int().$type<ChangedStateEnum>(),
  isCurrentActivity: int({ mode: 'boolean' }),
  startDateUtc: text(),
  startDateLocal: text(),
  duration: text(),
  distance: real(),
  changedAt: text()
    .notNull()
    .default(sql`(datetime('now'))`)
    .$onUpdateFn(() => new Date().toISOString()),
});
```
# DB2 Data Types for Your Schema

Let me give you the full picture of DB2 types, then translate your schema.

---

## DB2's Built-in Data Types

### Numeric

| DB2 Type | Size | Range | Notes |
|---|---|---|---|
| `SMALLINT` | 2 bytes | -32,768 to 32,767 | Like `int16` |
| `INTEGER` (or `INT`) | 4 bytes | -2.1B to 2.1B | Like `int32` |
| `BIGINT` | 8 bytes | ±9.2 × 10¹⁸ | Like `int64` |
| `DECIMAL(p,s)` | variable | up to 31 digits | Exact precision. For money. |
| `REAL` | 4 bytes | ~7 decimal digits | Single-precision float |
| `DOUBLE` | 8 bytes | ~15 decimal digits | Double-precision float |
| `DECFLOAT(16)` / `DECFLOAT(34)` | 8/16 bytes | decimal floating | IEEE 754-2008, LUW only |

### String

| DB2 Type | Notes |
|---|---|
| `CHAR(n)` | Fixed-length, space-padded. Length required. |
| `VARCHAR(n)` | Variable-length, max `n`. Length required. |
| `CLOB(n)` | Character Large OBject. For long text, up to 2GB. |
| `GRAPHIC(n)` / `VARGRAPHIC(n)` | Double-byte char sets (unusual in modern apps) |

**No `TEXT` type**. Use `VARCHAR(n)` with a chosen length, or `CLOB` for unbounded.

### Date/Time

| DB2 Type | Notes |
|---|---|
| `DATE` | Date only, no time |
| `TIME` | Time only, no date |
| `TIMESTAMP` | Date + time, sub-second precision up to 12 digits |
| `TIMESTAMP(6)` | Default; microsecond precision |
| `TIMESTAMP WITH TIME ZONE` | LUW only, includes TZ offset |

### Boolean

| DB2 Type | Notes |
|---|---|
| `BOOLEAN` | **LUW 11.1+ only**. Not on older i or older LUW. |

For portability, many projects use `SMALLINT` (0/1) or `CHAR(1)` (`'Y'`/`'N'`) instead of `BOOLEAN`.

### Binary

| DB2 Type | Notes |
|---|---|
| `BINARY(n)` / `VARBINARY(n)` | Fixed/variable binary, LUW 11+ |
| `BLOB(n)` | Binary Large OBject, up to 2GB |

### Specialty

| DB2 Type | Notes |
|---|---|
| `XML` | Native XML, both LUW and i |
| `ARRAY OF` | LUW only, in SQL PL contexts |

---

## What DB2 Does NOT Have

- **No `SERIAL` / `AUTOINCREMENT`** — use `GENERATED ALWAYS AS IDENTITY` on an `INTEGER` or `BIGINT` column.
- **No native `ENUM` type.** Use `VARCHAR(n)` + `CHECK` constraint.
- **No `JSON` type** (until very recent LUW; not on DB2 for i). Store as `CLOB` or `VARCHAR` and validate in the app.
- **No `UUID` type.** Use `CHAR(36)` for stringified UUIDs or `CHAR(16) FOR BIT DATA` / `BINARY(16)` for raw.

---

## Translating Your Schema

Your Drizzle schema uses SQLite conventions, which are loose. DB2 is stricter. Let me go through column by column.

### Understanding the types you're using

- `int()` in Drizzle = SQLite `INTEGER`, which is dynamically sized (1–8 bytes). You need to decide the DB2 size explicitly.
- `real()` = floating point. `REAL` (4 bytes) or `DOUBLE` (8 bytes) in DB2.
- `text()` = unlimited-length string in SQLite. In DB2 you must choose a length.
- `$type<DutyType>()` is Drizzle type-branding for TypeScript only; doesn't affect the stored type.
- `int({ mode: 'boolean' })` stores 0/1 as an integer but treats it as boolean in TS.
- `datetime('now')` is SQLite-specific.

### The Translation

```typescript
import { Kysely, sql } from 'kysely';

export async function up(db: Kysely<any>): Promise<void> {
  await db.schema
    .createTable('activities')
    // id as a client-generated string (UUID?); adjust length if you know the format
    .addColumn('id', 'varchar(36)', col => col.primaryKey())
    
    // These are enum-like integer codes in your app
    .addColumn('activity_duty_type', 'smallint')       // small enum, SMALLINT is plenty
    .addColumn('activity_code_name', 'varchar(100)')   // pick a sensible upper bound
    .addColumn('activity_code', 'integer')
    .addColumn('activity_id', 'integer')
    
    // Foreign keys (matching the referenced column types)
    .addColumn('address_id', 'varchar(36)', col =>
      col.references('addresses.id')
    )
    
    // Dates stored as ISO strings in your original — in DB2 use proper types
    .addColumn('date', 'date')                         // if it's date-only
    .addColumn('start_date_utc', 'timestamp')
    .addColumn('start_date_local', 'timestamp')
    
    .addColumn('driver_no', 'integer', col =>
      col.references('drivers.id')
    )
    .addColumn('vehicle_no', 'varchar(36)', col =>
      col.references('vehicles.id')
    )
    
    .addColumn('odometer_km', 'double')                // REAL would lose precision
    .addColumn('odometer_mi', 'double')
    
    .addColumn('context', 'smallint')                  // enum-like
    .addColumn('proximity_address', 'varchar(255)')
    .addColumn('change_state', 'smallint')             // enum-like
    
    // Boolean: SMALLINT 0/1 for portability (works on both LUW and i)
    .addColumn('is_current_activity', 'smallint', col =>
      col.check(sql`is_current_activity IN (0, 1)`)
    )
    
    .addColumn('duration', 'varchar(32)')              // ISO 8601 duration string?
    .addColumn('distance', 'double')
    
    .addColumn('changed_at', 'timestamp', col =>
      col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)
    )
    .execute();
}

export async function down(db: Kysely<any>): Promise<void> {
  await db.schema.dropTable('activities').execute();
}
```

---

## Key Translation Decisions Explained

### `id` as `VARCHAR(36)` vs `INTEGER IDENTITY`

Your original `id: text().primaryKey()` suggests a client-generated string (UUID or similar). In DB2:
- If it's always a UUID, `CHAR(36)` is better than `VARCHAR(36)` (fixed length, slightly more efficient).
- If it varies, `VARCHAR(36)` is safer.
- If you want DB-generated IDs instead, switch to:
  ```typescript
  .addColumn('id', 'integer', col => 
    col.primaryKey().generatedAlwaysAsIdentity()
  )
  ```

### Enum columns: `SMALLINT` vs `VARCHAR` + CHECK

Your enums (`activityDutyType`, `context`, `changeState`) are stored as integers in your SQLite schema. Keep them as `SMALLINT` in DB2 — it's 2 bytes and plenty for any realistic enum.

If you wanted string-based enums for readability:
```typescript
.addColumn('activity_duty_type', 'varchar(20)', col =>
  col.check(sql`activity_duty_type IN ('DRIVING', 'LOADING', 'RESTING')`)
)
```

The tradeoff: integer is smaller and faster; string is self-documenting at the database level. Your choice.

### Boolean as `SMALLINT`

I recommended `SMALLINT` with a CHECK for `is_current_activity`. Here's why over `BOOLEAN`:

- DB2 for i doesn't have native `BOOLEAN` at all on older TRs.
- DB2 for LUW supports `BOOLEAN` but many ODBC drivers still marshal it awkwardly (as integer, or as 'T'/'F', depending on config).
- `SMALLINT` with `CHECK (col IN (0,1))` is unambiguous across every DB2 version and driver.

In your app code, Kysely can provide a type bridge so you see `boolean` in TypeScript but write `0/1` to the column. Or you can cast at the boundary.

### Real vs Double for distance/odometer

Your original used `real()`. In DB2, `REAL` is single-precision (about 7 digits of accuracy). For an odometer reading, you'll accumulate error. I bumped to `DOUBLE` (15 digits). If you need exact precision (e.g., for reporting), consider `DECIMAL(10, 2)`.

### Dates as proper types

Your original stores dates as `text` (ISO strings). In DB2, use `DATE` for date-only columns and `TIMESTAMP` for date+time. Benefits:
- Proper comparison and arithmetic (`WHERE date > CURRENT DATE - 7 DAYS`).
- Indexed efficiently.
- Storage is smaller.
- Your Kysely types will reflect `Date` objects, not strings.

If your app really needs ISO strings for some reason, keep them as `VARCHAR(30)` — but this is almost always a mistake.

### `changed_at` default

`sql\`CURRENT TIMESTAMP\`` is DB2's equivalent of `datetime('now')`. Works on both LUW and i.

For `$onUpdateFn` (the "update this on every UPDATE" behavior), DB2 has two options:

1. **App-side**: always set `changed_at` explicitly in your update queries.
2. **DB-side** (DB2 LUW): use `GENERATED ALWAYS FOR EACH ROW ON UPDATE AS ROW CHANGE TIMESTAMP`:
   ```sql
   changed_at TIMESTAMP NOT NULL
     GENERATED ALWAYS FOR EACH ROW ON UPDATE AS ROW CHANGE TIMESTAMP
   ```
   The DB auto-sets this on every UPDATE. Very nice, LUW-only.
3. **Cross-platform**: a trigger.

For now, handle it in app code — it's the most portable option and simplest to reason about.

---

## Naming Convention Note

DB2 is case-insensitive for unquoted identifiers and folds everything to uppercase. Your Drizzle schema uses camelCase (`activityDutyType`), which DB2 would store as `ACTIVITYDUTYTYPE` — readable but ugly.

I converted to `snake_case` (`activity_duty_type`) in the translation. This is the DB2 convention and reads well when viewed in `isql` or any DB tool. Map between them in your Kysely type definitions:

```typescript
interface ActivitiesTable {
  id: string;
  activity_duty_type: number;
  // ...
}
```

Or use a Kysely plugin like `CamelCasePlugin` to convert at query time:
```typescript
const db = new Kysely<Database>({
  dialect: new Db2Dialect({...}),
  plugins: [new CamelCasePlugin()],
});
```

Then in TypeScript you write `activityDutyType`, and Kysely translates to `activity_duty_type` in the SQL.

---

## Before You Migrate

A few questions worth resolving before writing the migration:

1. **What type of ID is `id`?** UUID? Client-generated string? DB-generated integer? This changes `CHAR(36)` vs `INTEGER IDENTITY`.
2. **Are `DutyType`, `ActivityContext`, `ChangedStateEnum` small bounded sets (≤ a few dozen values)?** If yes, `SMALLINT` is fine. If they could grow to thousands, `INTEGER`.
3. **Do you care about referential integrity at the DB level?** The `.references(...)` in my translation assumes yes; the parent tables (`addresses`, `drivers`, `vehicles`) must exist *before* this migration runs, or the FK creation fails. Order your migration files accordingly.
4. **What's the `duration` column actually containing?** A seconds count (`INTEGER`)? An ISO 8601 duration string (`VARCHAR`)? DB2 has no native duration/interval type you'd normally use for this.

Answer these and the migration becomes concrete. Want me to review the final version once you've made the calls?

```
export const users = sqliteTable('users', {
  id: int().primaryKey({ autoIncrement: true }).notNull(),
  email: text().notNull().unique(),
  password: text().notNull(),
  role: text({ enum: ['admin', 'user'] }).notNull(),

  employeeNumber: int().unique(),
  firstName: text(),
  lastName: text(),

  createdAt: text()
    .notNull()
    .default(sql`(datetime('now'))`),
  changedAt: text()
    .notNull()
    .default(sql`(datetime('now'))`)
    .$onUpdateFn(() => new Date().toISOString()),
});
lets do this one next
```
# Translating the Users Table to DB2

Here's the DB2 version of your users schema. I'll walk through each decision.

---

## The Migration

```typescript

import { Kysely, sql } from 'kysely';

export async function up(db: Kysely<any>): Promise<void> {

await db.schema

.createTable('users')

.addColumn('id', 'integer', col =>

col.primaryKey().generatedAlwaysAsIdentity()

)

.addColumn('email', 'varchar(255)', col =>

col.notNull().unique()

)

.addColumn('password', 'varchar(255)', col =>

col.notNull()

)

.addColumn('role', 'varchar(10)', col =>

col.notNull().check(sql`role IN ('admin', 'user')`)

)

.addColumn('employee_number', 'integer', col =>

col.unique()

)

.addColumn('first_name', 'varchar(100)')

.addColumn('last_name', 'varchar(100)')

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.execute();

}

export async function down(db: Kysely<any>): Promise<void> {

await db.schema.dropTable('users').execute();

}

```

---

## Column-by-Column Decisions

### `id`: `INTEGER GENERATED ALWAYS AS IDENTITY`

```typescript

.addColumn('id', 'integer', col =>

col.primaryKey().generatedAlwaysAsIdentity()

)

```

Maps from SQLite's `int().primaryKey({ autoIncrement: true })`. DB2 has no `AUTOINCREMENT` keyword; the equivalent is `GENERATED ALWAYS AS IDENTITY`.

`GENERATED ALWAYS` means DB2 generates the value and rejects attempts to supply your own. If you ever need to insert a specific id (e.g., in data migrations), use `GENERATED BY DEFAULT AS IDENTITY` instead — it generates by default but lets you override.

**Watch this closely.** When you run the migration, inspect what SQL your dialect emits. If it produces `INTEGER AUTOINCREMENT` (SQLite-style) or `INTEGER SERIAL` (Postgres-style), your compiler has a gap and you'll need to fall back to raw SQL:

```typescript

await sql`

CREATE TABLE users (

id INTEGER NOT NULL GENERATED ALWAYS AS IDENTITY PRIMARY KEY,

...

)

`.execute(db);

```

Check this first, it's the most likely stumbling point.

### `email`: `VARCHAR(255) NOT NULL UNIQUE`

```typescript

.addColumn('email', 'varchar(255)', col =>

col.notNull().unique()

)

```

Length 255 is a common email column size. RFC 5321 says local-part ≤ 64 and domain ≤ 255, so technically emails could be up to 320 characters, but in practice 255 handles everything you'll see. Use 320 if you want to be pedantically correct.

`.unique()` creates a unique constraint. DB2 enforces this by auto-creating a unique index.

### `password`: `VARCHAR(255) NOT NULL`

```typescript

.addColumn('password', 'varchar(255)', col => col.notNull())

```

Length 255 is plenty for any modern hash:

- bcrypt: 60 chars

- argon2id encoded: ~97 chars

- scrypt encoded: varies but fits

I assume you're storing hashes, not plaintext. If you're ever tempted to store plaintext — don't.

### `role`: `VARCHAR(10)` with CHECK

```typescript

.addColumn('role', 'varchar(10)', col =>

col.notNull().check(sql`role IN ('admin', 'user')`)

)

```

DB2 has no enum type. The CHECK constraint restricts values to `'admin'` or `'user'`.

**Name the constraint explicitly** if you think you might change the allowed values later:

```typescript

.addCheckConstraint('users_role_check', sql`role IN ('admin', 'user')`)

```

Named constraints are easier to drop and recreate in a later migration. The `col.check(...)` form auto-generates a constraint name that's harder to reference.

Length 10 fits both values with room for a future addition (`'manager'`, `'guest'`). Don't make it too tight.

### `employee_number`: `INTEGER UNIQUE` (nullable)

```typescript

.addColumn('employee_number', 'integer', col => col.unique())

```

No `.notNull()` since your original allows null. DB2 `UNIQUE` constraints on nullable columns **allow multiple NULLs** (SQL standard behavior), which is usually what you want.

### `first_name` / `last_name`: `VARCHAR(100)` nullable

```typescript

.addColumn('first_name', 'varchar(100)')

.addColumn('last_name', 'varchar(100)')

```

100 chars is generous for names. You could go shorter (50) if storage matters, but names internationally get long — Spanish compound surnames, hyphenated names, etc. 100 is a safe default.

### `created_at`: `TIMESTAMP` with `CURRENT TIMESTAMP`

```typescript

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

```

Maps from SQLite's `datetime('now')`. DB2's equivalent is the `CURRENT TIMESTAMP` special register — no parentheses, no function call, it's a register read. Works identically on LUW and i.

Note I used a real `TIMESTAMP` column instead of `TEXT` storing an ISO string. This is a real improvement:

- Proper indexing and sort order.

- Date arithmetic: `WHERE created_at > CURRENT TIMESTAMP - 7 DAYS`.

- Smaller storage.

- Correct timezone handling (DB2 stores UTC by default; format on read).

### `changed_at`: the `$onUpdateFn` problem

```typescript

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

```

The default covers the INSERT case. But Drizzle's `$onUpdateFn` runs on every UPDATE — and DB2's `DEFAULT` only applies on INSERT.

Three ways to handle this. Pick one:

#### Option 1: Handle in app code (simplest, most portable)

Every time you UPDATE a user, explicitly set `changed_at`:

```typescript

await db.updateTable('users')

.set({

first_name: 'Bob',

changed_at: new Date(), // or sql`CURRENT TIMESTAMP`

})

.where('id', '=', 42)

.execute();

```

You could centralize this in a helper or a Kysely plugin that intercepts updates.

#### Option 2: `GENERATED ALWAYS FOR EACH ROW ON UPDATE AS ROW CHANGE TIMESTAMP` (DB2 LUW only)

DB2 has a built-in "row change timestamp" feature that auto-updates on every row modification:

```typescript

await sql`

ALTER TABLE users

ADD COLUMN changed_at TIMESTAMP NOT NULL

GENERATED ALWAYS FOR EACH ROW ON UPDATE AS ROW CHANGE TIMESTAMP

`.execute(db);

```

- Only works on LUW, and only when the table satisfies certain conditions.

- Not supported on DB2 for i in the same form (though i has `ROW CHANGE TIMESTAMP` in a different shape).

- Kysely's schema builder doesn't have a clean API for this, so raw SQL is likely needed.

#### Option 3: A trigger

```sql

CREATE TRIGGER users_changed_at

BEFORE UPDATE ON users

REFERENCING NEW AS n

FOR EACH ROW

SET n.changed_at = CURRENT TIMESTAMP;

```

Works on both platforms. Slightly invisible — future-you will see `changed_at` changing without any app code touching it, and wonder why.

**My recommendation: Option 1.** App-code explicitness is more portable, easier to debug, and adds very little burden. Mark it as a TODO to revisit if it becomes tedious.

---

## What to Watch When You Run This

1. **`generatedAlwaysAsIdentity` SQL output**: Does your dialect generate `GENERATED ALWAYS AS IDENTITY`? Log the compiled SQL or test with a throwaway `CREATE TABLE` in `isql`. If not, fall back to raw SQL for the id column.

2. **Check constraint SQL**: `col.check(sql`role IN (...)`)` — does your dialect emit this as a column-level `CHECK (role IN ('admin', 'user'))`? It should, but verify.

3. **The `CURRENT TIMESTAMP` default**: does it render as `DEFAULT CURRENT TIMESTAMP` (register, no parens) or `DEFAULT CURRENT_TIMESTAMP` (function-style with underscore, which also works on DB2)? Either should work, but SQL output inspection will confirm.

4. **The unique index creation**: `UNIQUE` on nullable `employee_number` creates a unique index allowing multiple NULLs. Verify after migration:

```sql

SELECT INDNAME, COLNAMES FROM SYSCAT.INDEXES WHERE TABNAME = 'USERS';

```

You should see indexes for the PK, email, and employee_number.

5. **Case of data in the `role` column**: Since DB2 is case-sensitive for string values (unlike identifiers), `'Admin'` ≠ `'admin'`. Make sure your app always writes lowercase. Or add a normalization trigger. Or add `'Admin'` to the allowed set if you want case-insensitive semantics (though that gets awkward).

---

## Kysely Type Definition

On the TypeScript side:

```typescript

interface UsersTable {

id: Generated<number>; // GENERATED ALWAYS AS IDENTITY

email: string;

password: string;

role: 'admin' | 'user'; // narrow type for the enum

employee_number: number | null;

first_name: string | null;

last_name: string | null;

created_at: Generated<Date>; // has DB default

changed_at: Generated<Date>; // has DB default on insert

}

interface Database {

users: UsersTable;

// ...

}

```

`Generated<T>` tells Kysely: "on insert, this is optional; on select, it's always present." Perfect for identity columns and timestamp defaults.

The `role` type is a literal union, not `string`. This gives you autocomplete and compile-time safety in queries like `where('role', '=', 'admin')`.

---

## Order Matters: Users Before Activities

In the previous migration translation for `activities`, you had FKs to `users` (via `driverNo`, though that references `drivers` not `users` — fair enough). But if any FK anywhere references `users.id`, **the users migration must run before that one**. Kysely runs migrations in filename order (alphabetical), so name your files with timestamps:

```

20240115000000_create_users.ts

20240115000100_create_addresses.ts

20240115000200_create_drivers.ts

20240115000300_create_vehicles.ts

20240115000400_create_activities.ts

```

Keep parent tables in earlier files than child tables with FKs.

---

## Ready to Run

Write the migration, run `pnpm migrate:up`, and inspect:

```sql

-- In isql:

SELECT * FROM users; -- should show no rows, correct columns

SELECT COLNAME, TYPENAME, LENGTH, NULLS

FROM SYSCAT.COLUMNS WHERE TABNAME = 'USERS'

ORDER BY COLNO; -- verify types

```

If anything looks off (wrong type, missing identity, check constraint not applied), paste the output and we'll figure out whether it's a schema compiler issue or something else. This migration is more complex than the smoke test, so it's a real exercise of your dialect.

```
great, next we can do this

import { sql } from 'drizzle-orm';

import { int, sqliteTable, text } from 'drizzle-orm/sqlite-core';

import { TrackingKeyEndpoints } from '../enums';

export const trackingKeysTable = sqliteTable('tracking_keys', {

endpoint: text().$type<TrackingKeyEndpoints>().primaryKey().notNull(),

trackingKey: int().notNull(),

createdAt: text()

.notNull()

.default(sql`(datetime('now'))`),

changedAt: text()

.notNull()

.default(sql`(datetime('now'))`)

.$onUpdateFn(() => new Date().toISOString()),

});

export enum TrackingKeyEndpoints {

TRIP_ASSIGNATION = 'TripAssignation',

ACTIVITY_RECORDS = 'ActivityRecords',

LOGBOOKS = 'Logbooks',

}
```
# Translating the Tracking Keys Table

Simple structure, but there's one interesting decision around the enum primary key. Let me walk through it.

---

## The Migration

```typescript

import { Kysely, sql } from 'kysely';

export async function up(db: Kysely<any>): Promise<void> {

await db.schema

.createTable('tracking_keys')

.addColumn('endpoint', 'varchar(50)', col =>

col.primaryKey()

.check(sql`endpoint IN ('TripAssignation', 'ActivityRecords', 'Logbooks')`)

)

.addColumn('tracking_key', 'integer', col => col.notNull())

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.execute();

}

export async function down(db: Kysely<any>): Promise<void> {

await db.schema.dropTable('tracking_keys').execute();

}

```

---

## Column-by-Column

### `endpoint`: `VARCHAR(50) PRIMARY KEY` with CHECK

```typescript

.addColumn('endpoint', 'varchar(50)', col =>

col.primaryKey()

.check(sql`endpoint IN ('TripAssignation', 'ActivityRecords', 'Logbooks')`)

)

```

This is the interesting one. Your original uses a string-based enum as the primary key. A few things to consider:

**Length choice.** Your current values are 'TripAssignation' (15), 'ActivityRecords' (15), 'Logbooks' (8). I went with `VARCHAR(50)` for headroom — if you add 'TruckInspectionReports' or similar in the future, you don't need to resize the column. 50 is enough for any reasonable endpoint name.

**Why keep the CHECK constraint.** Even though the PK already prevents duplicates, the CHECK enforces that only the three enumerated values are allowed. Without it, someone could insert `('Unknown', 42, ...)` and pollute your table. The constraint encodes domain logic.

**Alternative: don't use the enum as PK.** You could instead use:

```typescript

.addColumn('id', 'integer', col => col.primaryKey().generatedAlwaysAsIdentity())

.addColumn('endpoint', 'varchar(50)', col => col.notNull().unique().check(...))

```

Tradeoffs:

- String PK: human-readable, no join hop needed to see what endpoint, natural for this table's semantics.

- Integer PK: smaller index, better for large tables, more flexibility if endpoint names change.

For a table this small (3 rows, one per endpoint), the string PK is perfectly fine and more self-documenting. Keep it.

### `tracking_key`: `INTEGER NOT NULL`

```typescript

.addColumn('tracking_key', 'integer', col => col.notNull())

```

Direct port. Note: if `tracking_key` values can exceed ~2 billion, use `BIGINT` instead. Some tracking systems issue monotonically increasing identifiers that grow large over time, so consider whether `INTEGER` is really enough. If in doubt, `BIGINT` is cheap future-proofing.

### `created_at` / `changed_at`: `TIMESTAMP`

Same pattern as in the users table. `CURRENT TIMESTAMP` for the default, handle the `$onUpdateFn` in app code.

---

## Kysely Type Definition

```typescript

import { Generated } from 'kysely';

type TrackingKeyEndpoint = 'TripAssignation' | 'ActivityRecords' | 'Logbooks';

interface TrackingKeysTable {

endpoint: TrackingKeyEndpoint; // not Generated — you always insert it

tracking_key: number;

created_at: Generated<Date>;

changed_at: Generated<Date>;

}

interface Database {

users: UsersTable;

tracking_keys: TrackingKeysTable;

// ...

}

```

A note on the `TrackingKeyEndpoint` type: since you already have a TS enum `TrackingKeyEndpoints`, you could derive the union from it:

```typescript

import { TrackingKeyEndpoints } from './enums';

type TrackingKeyEndpoint = `${TrackingKeyEndpoints}`;

// resolves to 'TripAssignation' | 'ActivityRecords' | 'Logbooks'

```

The `${enum}` template literal trick extracts the string values from a TS enum. Neat and keeps the single source of truth.

---

## An Observation on This Table's Purpose

Looking at the shape — a small table with one row per endpoint, a numeric key, and timestamps — this looks like a **sync cursor / high-water-mark table**. You're tracking the last-synced identifier per endpoint, updating it every time you pull new data.

If that's the case, two patterns apply:

1. **Updates will be frequent, inserts rare.** Three rows are seeded once; `tracking_key` updates often. This makes the `changed_at` column genuinely useful for observability ("when did we last sync TripAssignation?").

2. **Consider seeding initial rows in the same migration.** Since the table is meaningless without its three rows, you might want:

```typescript

export async function up(db: Kysely<any>): Promise<void> {

await db.schema

.createTable('tracking_keys')

// ... columns

.execute();

await db.insertInto('tracking_keys')

.values([

{ endpoint: 'TripAssignation', tracking_key: 0 },

{ endpoint: 'ActivityRecords', tracking_key: 0 },

{ endpoint: 'Logbooks', tracking_key: 0 },

])

.execute();

}

```

**But recall the DDL+DML mixing caveat** from our earlier conversation: because `supportsTransactionalDdl = false`, if the INSERT fails after the CREATE succeeds, you're in a half-applied state. For seed data this simple, the risk is low, and you can recover by dropping the table manually if needed.

Alternatively, split into two migrations:

- `20240115000500_create_tracking_keys.ts` — just the CREATE TABLE.

- `20240115000600_seed_tracking_keys.ts` — just the INSERTs.

This is the more defensive pattern, and DML-only migrations are fully transactional, so the seed migration is safely atomic.

**My recommendation: split them.** The extra file is cheap, the isolation is worth it.

---

## The Seed Migration

```typescript

// 20240115000600_seed_tracking_keys.ts

import { Kysely } from 'kysely';

export async function up(db: Kysely<any>): Promise<void> {

await db.insertInto('tracking_keys')

.values([

{ endpoint: 'TripAssignation', tracking_key: 0 },

{ endpoint: 'ActivityRecords', tracking_key: 0 },

{ endpoint: 'Logbooks', tracking_key: 0 },

])

.execute();

}

export async function down(db: Kysely<any>): Promise<void> {

await db.deleteFrom('tracking_keys')

.where('endpoint', 'in', ['TripAssignation', 'ActivityRecords', 'Logbooks'])

.execute();

}

```

Pure DML. Fully transactional. If it fails, rolls back cleanly, you fix and rerun.

---

## Things to Verify After Running

```sql

SELECT * FROM tracking_keys;

-- Should show 3 rows after both migrations

SELECT COLNAME, TYPENAME, LENGTH, NULLS

FROM SYSCAT.COLUMNS WHERE TABNAME = 'TRACKING_KEYS'

ORDER BY COLNO;

-- Verify the check constraint exists:

SELECT CONSTNAME, TEXT FROM SYSCAT.CHECKS WHERE TABNAME = 'TRACKING_KEYS';

```

Ready to write both migrations. Same flags apply as before — if `col.check(sql\`...\`)` doesn't generate what you expect, fall back to `addCheckConstraint('tracking_keys_endpoint_check', sql\`...\`)` which uses a different code path in the schema compiler.

If your smoke test migration for users went cleanly, this one should too — it's less complex.
```
import { sql } from 'drizzle-orm';
import { real } from 'drizzle-orm/sqlite-core';
import { sqliteTable, text } from 'drizzle-orm/sqlite-core';

export const addressesTable = sqliteTable('addresses', {
  id: text().primaryKey().notNull(),
  name: text(),
  street: text(),
  city: text(),
  district: text(),
  country: text(),
  postalCode: text(),
  latitude: real(),
  longitude: real(),
  createdAt: text()
    .notNull()
    .default(sql`(datetime('now'))`),
  changedAt: text()
    .notNull()
    .default(sql`(datetime('now'))`)
    .$onUpdateFn(() => new Date().toISOString()),
});
```
# Translating the Addresses Table

Mostly straightforward, but the latitude/longitude columns deserve real thought.

---

## The Migration

```typescript
import { Kysely, sql } from 'kysely';

export async function up(db: Kysely<any>): Promise<void> {
  await db.schema
    .createTable('addresses')
    .addColumn('id', 'varchar(36)', col => col.primaryKey())
    .addColumn('name', 'varchar(200)')
    .addColumn('street', 'varchar(200)')
    .addColumn('city', 'varchar(100)')
    .addColumn('district', 'varchar(100)')
    .addColumn('country', 'varchar(100)')
    .addColumn('postal_code', 'varchar(20)')
    .addColumn('latitude', sql`DECIMAL(9, 6)`)
    .addColumn('longitude', sql`DECIMAL(9, 6)`)
    .addColumn('created_at', 'timestamp', col =>
      col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)
    )
    .addColumn('changed_at', 'timestamp', col =>
      col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)
    )
    .execute();
}

export async function down(db: Kysely<any>): Promise<void> {
  await db.schema.dropTable('addresses').execute();
}
```

---

## Column-by-Column

### `id`: `VARCHAR(36)` primary key

Same as the activities table — client-generated string, presumably UUID. If it's always a UUID, `CHAR(36)` is slightly more storage-efficient (fixed length); `VARCHAR(36)` is safer if the format could vary.

### Name and address components

I chose realistic lengths for each:

| Column | Length | Reasoning |
|---|---|---|
| `name` | 200 | Business names can be long: "Saint-Jean-Baptiste-de-la-Salle Medical Center Branch 3" |
| `street` | 200 | Full street lines with building numbers, apartments, etc. |
| `city` | 100 | Longest city names worldwide are ~85 chars. |
| `district` | 100 | Same as city. |
| `country` | 100 | Full country names: "The Democratic Republic of the Congo" is 37. |
| `postal_code` | 20 | UK postcodes can be 7 chars, Canadian 7, some systems include country code prefixes. 20 is generous. |

**Alternative: use `country` as a 2-letter ISO code.** `CHAR(2)` is way more efficient if you can commit to ISO 3166-1 alpha-2 codes ('US', 'CA', 'MX', etc.). Same for postal codes if you know the format. For a real address table you might normalize harder, but these loose lengths are fine for now.

**Watch out for `street` being nullable.** Your schema lets street be NULL. That's unusual — addresses without streets typically aren't valid addresses. But I kept it nullable to match your original. You may want to reconsider this at some point.

### `latitude` and `longitude`: `DECIMAL(9, 6)` — not REAL

This is the most important decision in this migration. Your Drizzle schema uses `real()` which is single-precision floating point. **Don't do this for coordinates.**

Here's why:

**`REAL` is single-precision float**: ~7 decimal digits of precision. A latitude like `40.7589234` gets stored as `40.75892` — you lose the last digit. That's about 1 meter of positional error near the equator, more near the poles.

**`DOUBLE` would be 15 digits of precision**: fine for coordinates, but floats have rounding quirks. `0.1 + 0.2 !== 0.3`. For GPS data this is usually acceptable, but if you ever do math like sum/average of coordinates, errors accumulate.

**`DECIMAL(9, 6)` gives you exactly 6 decimal places**: 
- 3 digits before the decimal (covers -180 to 180 for longitude, -90 to 90 for latitude).
- 6 digits after = about 11 cm precision at the equator. More than enough for any realistic application.
- No floating-point surprises. `40.758923` stored is `40.758923` retrieved, exactly.

This is the standard recommendation for geographic coordinates in a transactional database.

**Note on the syntax.** I used `sql\`DECIMAL(9, 6)\`` instead of a string like `'decimal(9, 6)'` because Kysely's built-in column types don't cleanly include DECIMAL with precision/scale. Using raw SQL for the type is the most reliable way. Depending on your dialect's compiler, you might also be able to use:

```typescript
.addColumn('latitude', 'decimal(9, 6)')
```

Test both and see which your dialect compiles correctly.

### `created_at` / `changed_at`

Same pattern as before. Nothing new.

---

## Kysely Type Definition

```typescript
interface AddressesTable {
  id: string;
  name: string | null;
  street: string | null;
  city: string | null;
  district: string | null;
  country: string | null;
  postal_code: string | null;
  latitude: string | null;         // ← note: DECIMAL comes back as a string via ODBC
  longitude: string | null;
  created_at: Generated<Date>;
  changed_at: Generated<Date>;
}
```

**Critical gotcha: DECIMAL types return as strings from ODBC.** The `node-odbc` driver returns `DECIMAL` columns as JavaScript strings, not numbers. This is intentional — it preserves exact precision that would be lost converting to `number` (which is a double internally).

So `latitude` in your TypeScript will be `"40.758923"`, not `40.758923`. You need to decide:

**Option A: Keep strings in the type, convert in app code.**
```typescript
const lat = parseFloat(address.latitude);
```
Simple, explicit, but every consumer needs to remember.

**Option B: Write a Kysely plugin that auto-converts DECIMAL columns.**
Intercept query results and `parseFloat` known DECIMAL columns before handing them back. More magic, less boilerplate, but adds a type-vs-runtime discrepancy if the conversion is wrong.

**Option C: Store coordinates as `DOUBLE` instead.**
Drops precision slightly but gives you real numbers in TS directly:
```typescript
.addColumn('latitude', 'double')
.addColumn('longitude', 'double')
```
Then `latitude: number | null` in TypeScript, no conversion needed.

My recommendation: **Option C for coordinates specifically.** The 15-digit precision of DOUBLE is more than enough for GPS coordinates (which are accurate to ~5 meters anyway from consumer devices). You gain native numeric types in TS and don't have to think about string-to-number conversion every time. `DECIMAL` is most valuable for money, not coordinates.

Revised migration for latitude/longitude:

```typescript
.addColumn('latitude', 'double')
.addColumn('longitude', 'double')
```

---

## Order in the Migration Sequence

This table has no FKs outgoing, but `activities.address_id` references `addresses.id`. So addresses must exist before activities:

```
20240115000000_create_users.ts
20240115000100_create_addresses.ts          ← here
20240115000200_create_drivers.ts
20240115000300_create_vehicles.ts
20240115000400_create_activities.ts
```

---

## Verify After Running

```sql
SELECT COLNAME, TYPENAME, LENGTH, SCALE, NULLS 
  FROM SYSCAT.COLUMNS WHERE TABNAME = 'ADDRESSES' 
  ORDER BY COLNO;
```

Check that `LATITUDE` and `LONGITUDE` come back as `DOUBLE` (or `DECIMAL` with the right scale if you went that route). That's the one thing most likely to surprise you.

---

## One More Thought

If you're planning to do geographic queries later ("find all addresses within 5km of a point"), DB2 LUW has spatial extensions (DB2 Spatial Extender) that provide proper `ST_POINT` types and spatial indexes. But those add complexity and aren't available on DB2 for i the same way.

For simple lat/lng storage and retrieval, plain `DOUBLE` columns are fine. If you do distance calculations client-side (using the Haversine formula in your app), you don't need spatial types at all. Only reach for spatial extensions when you need the database itself to do geographic indexing.

Ready for the next table when you are.

```
I forgot to mention that the incoming data for most tables is coming from ISAAC's APIs, which a TMS platform. Other tables will have data coming in from DB2 for I, before getting stored in an engineered manner within another db2 schema, which is going to be DB2 for LUW now, then DB2 for I

export const equipmentsTable = sqliteTable(
  'equipments',
  {
    id: text().primaryKey().notNull(),
    name: text(),
    equipmentTypeId: int(),
    licensePlate: text(),
    odometerAvailable: int({ mode: 'boolean' }),
    jurisdictionCode: text(),
    active: int({ mode: 'boolean' }),
    isOutOfService: int({ mode: 'boolean' }),
    creationUtcDate: text(),
    assetId: text(),
    siteId: int(),
    changedAt: text()
      .notNull()
      .default(sql`(datetime('now'))`)
      .$onUpdateFn(() => new Date().toISOString()),
  },
  (t) => [unique('name_license').on(t.name, t.licensePlate)],
);
```
# Translating the Equipments Table

Good context on the data source — that actually changes some decisions, which I'll flag as we go. Let me translate first, then discuss what ISAAC being upstream means.

---

## The Migration

```typescript

import { Kysely, sql } from 'kysely';

export async function up(db: Kysely<any>): Promise<void> {

await db.schema

.createTable('equipments')

.addColumn('id', 'varchar(36)', col => col.primaryKey())

.addColumn('name', 'varchar(100)')

.addColumn('equipment_type_id', 'integer')

.addColumn('license_plate', 'varchar(20)')

.addColumn('odometer_available', 'smallint', col =>

col.check(sql`odometer_available IN (0, 1)`)

)

.addColumn('jurisdiction_code', 'varchar(10)')

.addColumn('active', 'smallint', col =>

col.check(sql`active IN (0, 1)`)

)

.addColumn('is_out_of_service', 'smallint', col =>

col.check(sql`is_out_of_service IN (0, 1)`)

)

.addColumn('creation_utc_date', 'timestamp')

.addColumn('asset_id', 'varchar(50)')

.addColumn('site_id', 'integer')

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addUniqueConstraint('equipments_name_license_unique', ['name', 'license_plate'])

.execute();

}

export async function down(db: Kysely<any>): Promise<void> {

await db.schema.dropTable('equipments').execute();

}

```

---

## Column-by-Column Decisions

### `id`: `VARCHAR(36)` — same pattern

Presumably ISAAC's equipment identifier. Keep as VARCHAR(36) unless you know ISAAC uses shorter or longer IDs.

### `name`: `VARCHAR(100)`

Equipment names are usually human-readable labels like "Truck 42" or "Trailer A-104". 100 is generous.

### `equipment_type_id`: `INTEGER`

Foreign key-shaped, but no FK defined. If you later have an `equipment_types` table, you'd add `.references('equipment_types.id')`.

Worth asking: does ISAAC give you the equipment type as an ID, a name, or both? If the upstream API sends `{ equipmentType: "TRACTOR" }` you might want `VARCHAR(20)` instead. Your Drizzle schema uses `int` so I'm preserving that.

### `license_plate`: `VARCHAR(20)`

License plate formats vary wildly by jurisdiction:

- US state plates: 7-8 chars typical.

- European plates: can be 10+ with separators.

- Commercial vehicle plates: sometimes include region codes.

20 covers everything. You might see 15 elsewhere; I'd stay at 20 for safety.

### Three booleans: `SMALLINT` with CHECK

Same pattern as we used before. DB2 for i doesn't universally have `BOOLEAN`, and the ODBC layer is more predictable with SMALLINT 0/1.

For three booleans on the same table, you could factor the CHECK into a shared helper in your dialect later, but for now explicit is fine.

### `jurisdiction_code`: `VARCHAR(10)`

Jurisdictions for fleet are usually 2-letter (US states, Canadian provinces) or short codes ("CA-ON", "US-NY"). 10 is plenty.

### `creation_utc_date`: `TIMESTAMP` nullable

Note no default. Your original doesn't set a default and allows NULL — this value comes from ISAAC, not from your database.

**Subtle point**: since ISAAC provides this as "UTC date", and DB2 `TIMESTAMP` doesn't store timezone info, you're trusting that whatever you insert is already in UTC. Document this convention clearly. If you later mix UTC and local timestamps, you'll have a bad time.

### `asset_id`: `VARCHAR(50)`

No idea what format ISAAC uses. 50 should cover typical asset ID conventions (UUIDs, composite strings, etc.).

### `site_id`: `INTEGER`

Same comment as equipment_type_id. Shape suggests FK, but no reference defined.

### `changed_at`: `TIMESTAMP`

Same pattern as before. Handle `$onUpdateFn` in app code.

### The composite unique constraint

```typescript

.addUniqueConstraint('equipments_name_license_unique', ['name', 'license_plate'])

```

Your Drizzle `unique('name_license').on(t.name, t.licensePlate)` becomes a named table-level unique constraint.

**Heads up about NULLs in unique constraints**: DB2 allows multiple rows where either `name` or `license_plate` is NULL to coexist, because NULL is not equal to NULL in SQL. So this constraint enforces uniqueness of non-null (name, license_plate) pairs only.

If you need "only one row can have NULL name and NULL license_plate" semantics, that's much harder — not something CHECK or UNIQUE can express. You'd need triggers, or store a sentinel value instead of NULL, or ensure name and license_plate are always populated. Most likely your data has both populated anyway for real equipment, so this is a non-issue.

---

## What Changes Because ISAAC Is Upstream

You mentioned the data flows:

- ISAAC APIs → DB2 LUW (for now; eventually DB2 for i)

- DB2 for i → engineered schema in DB2 LUW (and back to i eventually)

This pattern — **upstream system of record, your DB is a synced mirror/warehouse** — has real implications for schema design. Let me call them out:

### 1. You don't own the primary keys

`equipments.id` is ISAAC's ID. You're storing it to correlate back. This means:

- **Don't use `GENERATED ALWAYS AS IDENTITY`.** You need to be able to write whatever ID ISAAC gives you. If you had used `GENERATED ALWAYS`, DB2 would reject your inserts.

- **If you ever need a local surrogate key**, add one alongside: `local_id INTEGER GENERATED ALWAYS AS IDENTITY`, but keep ISAAC's id as the PK or a unique column.

Your current approach (`VARCHAR(36) PRIMARY KEY`, app provides the value) is correct for this pattern.

### 2. Upsert semantics matter more than insert/update

When ISAAC changes an equipment record, you need to update your row, not duplicate it. DB2's equivalent of Postgres's `INSERT ... ON CONFLICT` is `MERGE`:

```sql

MERGE INTO equipments AS target

USING (VALUES (?, ?, ?, ...)) AS src(id, name, ...)

ON target.id = src.id

WHEN MATCHED THEN UPDATE SET name = src.name, ...

WHEN NOT MATCHED THEN INSERT (id, name, ...) VALUES (src.id, src.name, ...)

```

Kysely's query builder supports `mergeInto()`. This is how you'll sync ISAAC data. Worth noting now so you can structure your app code around it.

### 3. `changed_at` semantics get interesting

Normally `changed_at` means "when did *my app* last modify this row." When the row exists because ISAAC sent data, does `changed_at` mean:

- (a) When ISAAC last modified the record (comes from ISAAC's timestamp), or

- (b) When we last wrote it to our DB (set by us on each sync)?

These are different. If you mean (a), rename to `isaac_changed_at` and don't auto-update it on our UPDATEs — it should only change when ISAAC sends us a newer timestamp.

If you mean (b), keep current behavior, but be aware you can't use it to detect whether ISAAC has changed something.

Many sync systems store **both**: an `isaac_updated_at` (what ISAAC says) and a `synced_at` (when we last saw it). That way you can answer both "when was this data current per ISAAC?" and "when did we last confirm this row with ISAAC?"

Worth thinking about as you design the sync logic. No schema change required today, but you may want to add `isaac_updated_at` in a future migration.

### 4. Nullable columns reflect upstream nullability

Most of your columns are nullable because ISAAC might not send them. That's correct. But it's worth documenting which columns are *required by ISAAC* (so they should be NOT NULL even though the raw API allows null) vs. genuinely optional. This prevents bugs like "we stored an equipment with no name because the ISAAC response was malformed."

### 5. Consider a `raw_payload` column for debugging

When syncing from an external API, a common pattern is:

```typescript

.addColumn('raw_payload', 'clob') // stringified JSON of what ISAAC sent

.addColumn('synced_at', 'timestamp') // when we last saved this payload

```

Valuable for debugging when ISAAC's schema changes or when something parses weirdly. You can always regenerate your structured columns from the raw payload. Optional, and adds storage cost, but often worth it for integration tables.

---

## Kysely Type Definition

```typescript

interface EquipmentsTable {

id: string; // from ISAAC, not generated

name: string | null;

equipment_type_id: number | null;

license_plate: string | null;

odometer_available: number | null; // 0 | 1, use a helper to convert to boolean

jurisdiction_code: string | null;

active: number | null;

is_out_of_service: number | null;

creation_utc_date: Date | null;

asset_id: string | null;

site_id: number | null;

changed_at: Generated<Date>;

}

```

The boolean-as-smallint pattern is ugly in TypeScript. You'll want conversion helpers:

```typescript

function toBool(n: number | null): boolean | null {

if (n === null) return null;

return n === 1;

}

function fromBool(b: boolean | null): number | null {

if (b === null) return null;

return b ? 1 : 0;

}

```

Use on read/write boundaries. Or, again, write a Kysely plugin that auto-converts. Either works.

---

## Order in the Migration Sequence

Equipments don't reference anyone, and aren't referenced by the tables we've translated so far. But if `activities.vehicle_no` or similar ends up being `equipments.id`, you'd order equipments before activities. Looks like your current setup has separate `vehicles` and `drivers` tables that activities references, so equipments might live on its own.

```

20240115000000_create_users.ts

20240115000100_create_addresses.ts

20240115000200_create_equipments.ts ← here, no dependencies

20240115000300_create_drivers.ts

20240115000400_create_vehicles.ts

20240115000500_create_activities.ts

20240115000600_create_tracking_keys.ts

20240115000700_seed_tracking_keys.ts

```

---

## Verify After Running

```sql

SELECT COLNAME, TYPENAME, LENGTH, NULLS

FROM SYSCAT.COLUMNS WHERE TABNAME = 'EQUIPMENTS'

ORDER BY COLNO;

-- Verify the composite unique:

SELECT CONSTNAME, COLNAMES

FROM SYSCAT.INDEXES WHERE TABNAME = 'EQUIPMENTS';

-- Should show an index for the unique constraint covering both columns.

-- Verify check constraints:

SELECT CONSTNAME, TEXT FROM SYSCAT.CHECKS WHERE TABNAME = 'EQUIPMENTS';

```

---

## A Question for You

Given ISAAC is upstream, are you planning to:

1. **Mirror ISAAC tables 1:1** (each ISAAC entity → one table with same columns)?

2. **Transform into a domain model** (one or more ISAAC tables → a cleaner domain table)?

If (1), the translations I'm giving you are close to final. If (2), these tables are intermediate and you might want to mark them as such (`raw_equipments`, `staging_equipments`, or put them in a separate schema like `STAGING`). The "engineered" version you mentioned sits on top.

This is worth deciding architecturally before you accumulate too many tables, because retrofitting a staging/domain separation later is painful. Happy to think through it with you when you're ready.

Ready for the next one.

```
ok, let me share everything else in this prompt

import { activitiesTable } from '../tables/activities';

export const activityFormsTable = sqliteTable('activity_forms', {

id: text().primaryKey().notNull(),

activityRecordId: text().references(() => activitiesTable.id),

key: text(),

value: text(),

createdAt: text()

.notNull()

.default(sql`(datetime('now'))`),

changedAt: text()

.notNull()

.default(sql`(datetime('now'))`)

.$onUpdateFn(() => new Date().toISOString()),

});

import { sql } from 'drizzle-orm';

import { text } from 'drizzle-orm/sqlite-core';

import { real } from 'drizzle-orm/sqlite-core';

import { sqliteTable } from 'drizzle-orm/sqlite-core';

import { int } from 'drizzle-orm/sqlite-core';

export const driversTable = sqliteTable('drivers', {

// ── Identity (Operator.operatorNo = Employee.EMPLOYEE_NUMBER) ──────────────

id: int().primaryKey().unique(),

// ── Name ──────────────────────────────────────────────────────────────────

firstName: text(),

lastName: text(),

// ── Contact ───────────────────────────────────────────────────────────────

email: text(),

phoneNumber: int(),

phoneNumberMobile: int(),

phoneNumberEmergency: int(),

emergencyContact: text(),

// ── Dates ─────────────────────────────────────────────────────────────────

birthDate: text(),

startDate: text(),

hiredDateEnd: text(),

rehiredDateStart: text(),

rehiredDateEnd: text(),

seniorityDate: text(),

seniorityRank: text(),

onTrialEndDate: text(),

onTrialDaysLeft: int(),

// ── Status / Activity ─────────────────────────────────────────────────────

active: int({ mode: 'boolean' }),

employeeActivityCode: text(),

employeeStatusCode: text(),

employeeTypeCode: text(),

employeeCategoryCode: text(),

// ── Licence ───────────────────────────────────────────────────────────────

licenceNumber: text(),

licenceReference: text(),

licenceClassCode: text(),

licenceExpiryDate: text(),

classObtainedDate: text(),

licenceMention1Code: text(),

licenceMention2Code: text(),

licenceMention3Code: text(),

licenceCondition1Code: text(),

licenceCondition2Code: text(),

licenceCondition3Code: text(),

jurisdictionCode: text(),

// ── Medical / Compliance ───────────────────────────────────────────────────

hazmatExpiryDate: text(),

medicalDueDate: text(),

socialNumber: int(),

// ── HOS / ELD (Operator-only) ─────────────────────────────────────────────

hosRuleTypeCanNorth: int(),

hosRuleTypeCan: int(),

hosRuleTypeUs: int(),

showHos: int({ mode: 'boolean' }),

eldCompliant: int({ mode: 'boolean' }),

eldExempted: int({ mode: 'boolean' }),

eldExemptReason: text(),

eldCanCompliant: int({ mode: 'boolean' }),

eldCanExempted: int({ mode: 'boolean' }),

eldCanExemptReason: text(),

// ── Exceptions (Operator-only) ────────────────────────────────────────────

adverseConditionEnabled: int({

mode: 'boolean',

}),

sixteenHourException: int({ mode: 'boolean' }),

restDeferralEnabled: int({ mode: 'boolean' }),

livestockExceptionEnabled: int({

mode: 'boolean',

}),

oversizeOverweightException: int({

mode: 'boolean',

}),

agriculturalException: int({ mode: 'boolean' }),

yardMoveEnabled: int({ mode: 'boolean' }),

personalConveyanceEnabled: int({

mode: 'boolean',

}),

sleeperBerthAnticipation: int({

mode: 'boolean',

}),

// ── Dispatch / Org (Operator-only) ────────────────────────────────────────

avgRateByKm: real(),

classificationTypeId: int(),

cultureId: int(),

defaultDispatchGroupId: int(),

siteId: int(),

activityGroupId: int(),

dayStartTime: text(),

// ── Employee Org (Employee-only) ──────────────────────────────────────────

terminalCode: text(),

terminalWorkPlace: text(),

companyCode: text(),

agencyNumber: int(),

baseCode: text(),

employeeTitle: text(),

genderCode: text(),

languageCode: text(),

unionizedFlag: text(),

userProfile1: text(),

userProfile2: text(),

createdAt: text()

.notNull()

.default(sql`(datetime('now'))`),

changedAt: text()

.notNull()

.default(sql`(datetime('now'))`)

.$onUpdateFn(() => new Date().toISOString()),

});

import { sql } from 'drizzle-orm';

import { int, sqliteTable, text } from 'drizzle-orm/sqlite-core';

import { activitiesTable } from './activities';

import { driversTable } from './drivers';

export const fdrsTable = sqliteTable('fdrs', {

id: int().primaryKey({ autoIncrement: true }).notNull(),

connexion: text()

.references(() => activitiesTable.id)

.notNull()

.unique(),

deconnexion: text()

.references(() => activitiesTable.id)

.unique(),

driverNo: int().references(() => driversTable.id),

createdAt: text()

.notNull()

.default(sql`(datetime('now'))`),

changedAt: text()

.notNull()

.default(sql`(datetime('now'))`)

.$onUpdateFn(() => new Date().toISOString()),

});

import { sql } from 'drizzle-orm';

import { sqliteTable, text } from 'drizzle-orm/sqlite-core';

export const seedingTable = sqliteTable('seeding', {

id: text().primaryKey().default('singleton').notNull(),

date: text(),

createdAt: text()

.notNull()

.default(sql`(datetime('now'))`),

changedAt: text()

.notNull()

.default(sql`(datetime('now'))`)

.$onUpdateFn(() => new Date().toISOString()),

});

import { sql } from 'drizzle-orm';

import { int, real, sqliteTable, text } from 'drizzle-orm/sqlite-core';

import { ProgressStateEnum } from '../enums';

import { addressesTable } from './addresses';

import { tripsTable } from './trips';

export const stopsTable = sqliteTable('stops', {

id: text().primaryKey().notNull(),

version: int(),

tripId: text().references(() => tripsTable.id),

type: text(),

client: text(),

addressId: text().references(() => addressesTable.id),

estimatedDepartureTime: text(),

estimatedArrivalTime: text(),

note: text(),

distanceFromPreviousStop: real(),

active: int({ mode: 'boolean' }).default(true),

preArrivalRadius: real(),

stopRadius: real(),

extraInfo: text(),

usAgriculturalExemptionType: int().$type<ProgressStateEnum>(),

copilotProfileId: int(),

progressState: int().$type<ProgressStateEnum>(),

createdAt: text()

.notNull()

.default(sql`(datetime('now'))`),

finishedAt: text(),

});

export const waypoints = sqliteTable('waypoints', {

id: text().primaryKey().notNull(),

stopId: text().references(() => stopsTable.id),

latitude: real(),

longitude: real(),

});

import { sql } from 'drizzle-orm';

import { int, sqliteTable, text } from 'drizzle-orm/sqlite-core';

import { ProgressStateEnum } from '../enums';

import { stopsTable } from './stops';

export const tasksTable = sqliteTable('tasks', {

id: text().primaryKey().notNull(),

stopId: text().references(() => stopsTable.id),

progressState: int().$type<ProgressStateEnum>(),

activityId: int(),

version: int(),

name: text(),

active: int({ mode: 'boolean' }),

metaDataId: text(),

externalValidation: int({ mode: 'boolean' }),

createdAt: text()

.notNull()

.default(sql`(datetime('now'))`),

finishedAt: text(),

});

import { int, real, sqliteTable, text } from 'drizzle-orm/sqlite-core';

import { ProgressStateEnum } from '../enums';

import { driversTable } from './drivers';

import { vehiclesTable } from './vehicles';

export const tripsTable = sqliteTable('trips', {

id: text().primaryKey().notNull(),

version: int(),

tripNo: text(),

reference: text(),

note: text(),

assignationDate: text(),

distance: real(),

startDate: text(),

vehicleNo: text().references(() => vehiclesTable.id),

active: int({ mode: 'boolean' }).default(true),

copilotProfileId: int(),

sequenceNo: int(),

progressState: int().$type<ProgressStateEnum>(),

driverNo: int().references(() => driversTable.id),

finishedAt: text(),

});

import { sql } from 'drizzle-orm';

import { int, sqliteTable, text } from 'drizzle-orm/sqlite-core';

export const vehiclesTable = sqliteTable('vehicles', {

id: text().primaryKey(),

siteId: int().notNull(),

vehicleStateId: int().notNull(),

vehicleTypeId: int().notNull(),

engineTypeId: int().notNull(),

modelYear: int().notNull(),

description: text().notNull(),

serialNo: text().notNull(),

defaultOperatorNo: int(),

licensePlate: text().notNull(),

dtcGroupId: int().notNull(),

vehicleGroupId: int().notNull(),

jurisdictionCode: text().notNull(),

eldCompliant: int({ mode: 'boolean' }).notNull(),

emergencyMessagesEnabled: int({ mode: 'boolean' }).notNull(),

active: int({ mode: 'boolean' }).notNull(),

assetId: text().notNull().unique(),

isOutOfService: int({ mode: 'boolean' }).notNull(),

activityGroupId: int().notNull(),

recorderInstalled: int({ mode: 'boolean' }).notNull(),

gatewaySerialNo: int(),

createdAt: text()

.notNull()

.default(sql`(datetime('now'))`),

changedAt: text()

.notNull()

.default(sql`(datetime('now'))`)

.$onUpdateFn(() => new Date().toISOString()),

});

export enum ProgressStateEnum {

NOT_STARTED = 0,

IN_PROGRESS = 1,

DONE = 2,

}

export enum DutyType {

NONE = 0,

ON_DUTY = 1,

DRIVING = 2,

OFF_DUTY = 3,

SLEEPER = 4,

}

export enum ActivityContext {

ZERO = 0,

ONE = 1,

TWO = 2,

FOUR = 4,

}

export enum ChangedStateEnum {

NEW = 1,

UPDATED = 2,

DELETED = 3,

}

// FIX: RULE TYPE IS NOT YET DEFINED

export enum RuleType {

ZERO = 0,

ONE = 1,

TWO = 2,

THREE = 3,

FOUR = 4,

FIVE = 5,

SIX = 6,

SEVEN = 7,

EIGHT = 8,

NINE = 9,

TEN = 10,

HUNDRED = 100,

ONE_TEN = 110,

ONE_TWENTY = 120,

ONE_THIRTY = 130,

TWO_HUNDRED = 200,

TWO_O_ONE = 201,

TWO_TWENTY_FIVE = 225,

TWO_TWENTY_SIX = 226,

TWO_TWENTY_SEVEN = 227,

}

export enum TrackingKeyEndpoints {

TRIP_ASSIGNATION = 'TripAssignation',

ACTIVITY_RECORDS = 'ActivityRecords',

LOGBOOKS = 'Logbooks',

}

import { createHash } from 'crypto';

import { AddressInfo } from '@/isaac/types';

function normalize(value?: string | null) {

return (value ?? '').trim();

}

export function getAddressId(address: AddressInfo) {

return createHash('sha256')

.update(

[

normalize(address.street),

normalize(address.name),

normalize(address.city),

normalize(address.district),

normalize(address.country),

normalize(address.postalCode),

normalize(address.longitude.toString()),

normalize(address.longitude.toString()),

].join('|'),

)

.digest('hex');

}

export const MAX_INSERT_ROWS = 999;

import { getTableColumns, SQL, sql } from 'drizzle-orm';

import { toSnakeCase } from 'drizzle-orm/casing';

import { SQLiteTable } from 'drizzle-orm/sqlite-core';

import { SQLiteColumn } from 'drizzle-orm/sqlite-core';

import { getTableConfig } from 'drizzle-orm/sqlite-core';

import { dbSqlite } from '../connection';

import { MAX_INSERT_ROWS } from './constants';

type ColumnMode = 'update' | 'ignore' | 'fill_null';

export function buildConflictUpdateColumns<

T extends SQLiteTable,

Q extends keyof T['_']['columns'],

>(table: T, modes: Partial<Record<Q, ColumnMode>> = {}) {

const cls = getTableColumns(table);

const { name: tableName } = getTableConfig(table);

return Object.fromEntries(

Object.entries(cls)

.filter(([key]) => modes[key as Q] !== 'ignore')

.map(([key]) => {

const mode = modes[key as Q] ?? 'update';

const sqlName = toSnakeCase(key);

const excluded = sql.raw(`excluded.${sqlName}`);

const existing = sql.raw(`"${tableName}"."${sqlName}"`);

const value =

mode === 'fill_null'

? sql`CASE WHEN ${existing} IS NULL THEN ${excluded} ELSE ${existing} END`

: sql`CASE WHEN ${excluded} IS NOT NULL THEN ${excluded} ELSE ${existing} END`;

return [key, value];

}),

) as Record<Q, SQL>;

}

export async function chunkedInsertOnConflict<T extends SQLiteTable>(

table: T,

data: T['$inferInsert'][],

target: SQLiteColumn,

modes: Partial<Record<keyof T['_']['columns'], ColumnMode>> = {},

) {

const set = buildConflictUpdateColumns(table, modes);

const chunks = Array.from(

{ length: Math.ceil(data.length / MAX_INSERT_ROWS) },

(_, i) => data.slice(i * MAX_INSERT_ROWS, (i + 1) * MAX_INSERT_ROWS),

);

return (

await Promise.all(

chunks.map((chunk) =>

dbSqlite

.insert(table)

.values(chunk)

.onConflictDoUpdate({ target, set })

.returning(),

),

)

).flat();

}

export async function chunkedInsertDoNothing<T extends SQLiteTable>(

table: T,

data: T['$inferInsert'][],

) {

const chunks = Array.from(

{ length: Math.ceil(data.length / MAX_INSERT_ROWS) },

(_, i) => data.slice(i * MAX_INSERT_ROWS, (i + 1) * MAX_INSERT_ROWS),

);

await Promise.all(

chunks.map((chunk) =>

dbSqlite.insert(table).values(chunk).onConflictDoNothing(),

),

);

}
```

# Translating the Remaining Tables

A lot to go through. I'll focus on the non-obvious decisions per table and suggest a migration ordering. I'll also flag the helpers at the end because they'll need real thought on the DB2 side.

---

## First: Reordering the Migrations

Dependencies now:

```

users (no deps)

addresses (no deps)

equipments (no deps)

drivers (no deps)

vehicles (no deps)

tracking_keys (no deps)

tracking_keys_seed (depends on tracking_keys)

seeding (no deps)

trips (→ drivers, vehicles)

stops (→ trips, addresses)

waypoints (→ stops)

tasks (→ stops)

activities (→ addresses, drivers, vehicles)

activity_forms (→ activities)

fdrs (→ activities, drivers)

```

Ordered file names:

```

20240115000000_create_users.ts

20240115000100_create_addresses.ts

20240115000200_create_equipments.ts

20240115000300_create_drivers.ts

20240115000400_create_vehicles.ts

20240115000500_create_tracking_keys.ts

20240115000600_seed_tracking_keys.ts

20240115000700_create_seeding.ts

20240115000800_create_trips.ts

20240115000900_create_stops.ts

20240115001000_create_waypoints.ts

20240115001100_create_tasks.ts

20240115001200_create_activities.ts

20240115001300_create_activity_forms.ts

20240115001400_create_fdrs.ts

```

---

## Activity Forms

```typescript

await db.schema

.createTable('activity_forms')

.addColumn('id', 'varchar(36)', col => col.primaryKey())

.addColumn('activity_record_id', 'varchar(36)', col =>

col.references('activities.id')

)

.addColumn('key', 'varchar(100)')

.addColumn('value', 'varchar(4000)') // see note below

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.execute();

```

**The `value` column length is a real decision.** Form values can be anything — short answers, long text, JSON payloads. Options:

- `VARCHAR(4000)`: safe for most free-text. Fits in a row without overflow on DB2 LUW.

- `CLOB`: unlimited text up to 2GB. Use if forms can contain long blobs.

- `VARCHAR(32672)`: DB2's max VARCHAR size (with extended row size enabled, otherwise 32,704).

Without knowing your form content, `VARCHAR(4000)` is a reasonable default. If you hit size errors, bump to CLOB.

---

## Drivers

This is the big one. 80+ columns. Let me address the patterns rather than every column.

### Pattern 1: Phone numbers as integers — don't

Your schema has `phoneNumber: int()`, `phoneNumberMobile: int()`, etc. **Phone numbers should never be integers.** Reasons:

- Leading zeros get stripped (European numbers often start with 0).

- International prefixes and country codes don't fit in `INTEGER` (max ~2B).

- Extensions like "555-1234 ext 42" can't be represented.

- Formatting (dashes, parens, spaces) is lost.

Store phone numbers as `VARCHAR(30)`. If you want to normalize for queries, store both a raw and an E.164-formatted version. But never as integers.

I'll translate them as VARCHAR despite your Drizzle schema saying `int`. This is a correctness fix, not a stylistic change.

### Pattern 2: `social_number` as integer — also don't

Same reasoning. Social security / insurance numbers often have leading zeros and sometimes formatting. `VARCHAR(20)` is the right choice. Also, be aware this is sensitive PII — consider whether it should be encrypted at rest or stored at all.

### Pattern 3: Date columns that are really dates

Your original stores them as `text` (ISO strings). Some are clearly dates (`birthDate`, `startDate`, `hiredDateEnd`), others might be date or timestamp (`dayStartTime` might be "08:00" for a daily start time — a `TIME` type).

I'll use `DATE` for clearly-date columns, `TIME` for `dayStartTime`, and `TIMESTAMP` for anything ambiguous that might include time.

### Pattern 4: The boolean explosion

About 20 boolean columns. All become `SMALLINT` with `CHECK (col IN (0, 1))`. This gets verbose in the migration. Consider a helper in your migration file:

```typescript

function boolColumn(

b: CreateTableBuilder,

name: string,

): CreateTableBuilder {

return b.addColumn(name, 'smallint', col =>

col.check(sql`${sql.ref(name)} IN (0, 1)`)

);

}

```

Use as:

```typescript

let b = db.schema.createTable('drivers').addColumn('id', 'integer', ...);

b = boolColumn(b, 'active');

b = boolColumn(b, 'show_hos');

// ...

await b.execute();

```

Less repetition. The `sql.ref(name)` ensures the column name is properly escaped in the check constraint.

### Pattern 5: `id` is the employee number, not a UUID

```typescript

id: int().primaryKey().unique(),

```

This is meaningful — the driver's ID is their employee number, an integer coming from the upstream system. So:

```typescript

.addColumn('id', 'integer', col => col.primaryKey())

```

No `GENERATED AS IDENTITY`, because the value comes from outside.

### The Drivers Migration (condensed)

```typescript

await db.schema

.createTable('drivers')

// Identity

.addColumn('id', 'integer', col => col.primaryKey())

// Name

.addColumn('first_name', 'varchar(100)')

.addColumn('last_name', 'varchar(100)')

// Contact — phone numbers as VARCHAR

.addColumn('email', 'varchar(255)')

.addColumn('phone_number', 'varchar(30)')

.addColumn('phone_number_mobile', 'varchar(30)')

.addColumn('phone_number_emergency', 'varchar(30)')

.addColumn('emergency_contact', 'varchar(200)')

// Dates

.addColumn('birth_date', 'date')

.addColumn('start_date', 'date')

.addColumn('hired_date_end', 'date')

.addColumn('rehired_date_start', 'date')

.addColumn('rehired_date_end', 'date')

.addColumn('seniority_date', 'date')

.addColumn('seniority_rank', 'varchar(10)')

.addColumn('on_trial_end_date', 'date')

.addColumn('on_trial_days_left', 'integer')

// Status

.addColumn('active', 'smallint', col => col.check(sql`active IN (0, 1)`))

.addColumn('employee_activity_code', 'varchar(10)')

.addColumn('employee_status_code', 'varchar(10)')

.addColumn('employee_type_code', 'varchar(10)')

.addColumn('employee_category_code', 'varchar(10)')

// Licence

.addColumn('licence_number', 'varchar(30)')

.addColumn('licence_reference', 'varchar(30)')

.addColumn('licence_class_code', 'varchar(10)')

.addColumn('licence_expiry_date', 'date')

.addColumn('class_obtained_date', 'date')

.addColumn('licence_mention1_code', 'varchar(10)')

.addColumn('licence_mention2_code', 'varchar(10)')

.addColumn('licence_mention3_code', 'varchar(10)')

.addColumn('licence_condition1_code', 'varchar(10)')

.addColumn('licence_condition2_code', 'varchar(10)')

.addColumn('licence_condition3_code', 'varchar(10)')

.addColumn('jurisdiction_code', 'varchar(10)')

// Medical

.addColumn('hazmat_expiry_date', 'date')

.addColumn('medical_due_date', 'date')

.addColumn('social_number', 'varchar(20)')

// HOS/ELD

.addColumn('hos_rule_type_can_north', 'integer')

.addColumn('hos_rule_type_can', 'integer')

.addColumn('hos_rule_type_us', 'integer')

.addColumn('show_hos', 'smallint', col => col.check(sql`show_hos IN (0, 1)`))

.addColumn('eld_compliant', 'smallint', col => col.check(sql`eld_compliant IN (0, 1)`))

.addColumn('eld_exempted', 'smallint', col => col.check(sql`eld_exempted IN (0, 1)`))

.addColumn('eld_exempt_reason', 'varchar(200)')

.addColumn('eld_can_compliant', 'smallint', col => col.check(sql`eld_can_compliant IN (0, 1)`))

.addColumn('eld_can_exempted', 'smallint', col => col.check(sql`eld_can_exempted IN (0, 1)`))

.addColumn('eld_can_exempt_reason', 'varchar(200)')

// Exceptions (all SMALLINT 0/1, CHECKs omitted for brevity — add them)

.addColumn('adverse_condition_enabled', 'smallint')

.addColumn('sixteen_hour_exception', 'smallint')

.addColumn('rest_deferral_enabled', 'smallint')

.addColumn('livestock_exception_enabled', 'smallint')

.addColumn('oversize_overweight_exception', 'smallint')

.addColumn('agricultural_exception', 'smallint')

.addColumn('yard_move_enabled', 'smallint')

.addColumn('personal_conveyance_enabled', 'smallint')

.addColumn('sleeper_berth_anticipation', 'smallint')

// Dispatch/Org

.addColumn('avg_rate_by_km', 'double')

.addColumn('classification_type_id', 'integer')

.addColumn('culture_id', 'integer')

.addColumn('default_dispatch_group_id', 'integer')

.addColumn('site_id', 'integer')

.addColumn('activity_group_id', 'integer')

.addColumn('day_start_time', 'time')

// Employee Org

.addColumn('terminal_code', 'varchar(10)')

.addColumn('terminal_work_place', 'varchar(50)')

.addColumn('company_code', 'varchar(10)')

.addColumn('agency_number', 'integer')

.addColumn('base_code', 'varchar(10)')

.addColumn('employee_title', 'varchar(50)')

.addColumn('gender_code', 'varchar(5)')

.addColumn('language_code', 'varchar(5)')

.addColumn('unionized_flag', 'varchar(5)')

.addColumn('user_profile1', 'varchar(50)')

.addColumn('user_profile2', 'varchar(50)')

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.execute();

```

I omitted CHECK constraints on most of the exception booleans for brevity — add them in your real migration, or accept slightly looser validation if you trust the source data.

### About the 9-column table limit myth

You might worry about DB2 having column limits. It doesn't — the practical limit is 1012 per table on LUW, 8000 on i. 80 columns is nothing. But:

**Row size is a real limit.** On LUW, the default row size limit is 4,005 bytes (based on page size 4K). With all these VARCHARs, you could exceed it. If the CREATE TABLE fails with `SQL0670N` (row size too large), you have two options:

1. Use a larger tablespace page size (up to 32K).

2. Reduce VARCHAR lengths.

This table's real row size is hard to estimate without doing the math — VARCHAR only stores actual content plus 2 bytes overhead, so in practice you're fine unless every field is maxed out. If you hit the error, let me know and we'll adjust.

---

## FDRs

```typescript

await db.schema

.createTable('fdrs')

.addColumn('id', 'integer', col =>

col.primaryKey().generatedAlwaysAsIdentity()

)

.addColumn('connexion', 'varchar(36)', col =>

col.notNull().unique().references('activities.id')

)

.addColumn('deconnexion', 'varchar(36)', col =>

col.unique().references('activities.id')

)

.addColumn('driver_no', 'integer', col =>

col.references('drivers.id')

)

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.execute();

```

`id` is DB-generated here (your original uses `autoIncrement: true`), so `GENERATED ALWAYS AS IDENTITY` is right.

---

## Seeding

```typescript

await db.schema

.createTable('seeding')

.addColumn('id', 'varchar(20)', col =>

col.primaryKey().defaultTo('singleton')

)

.addColumn('date', 'timestamp')

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.execute();

```

The singleton pattern. To enforce "only one row ever," you could add:

```typescript

.addCheckConstraint('seeding_singleton', sql`id = 'singleton'`)

```

Now even if someone tries to insert a different ID, DB2 blocks it. Makes the "singleton" intent explicit at the schema level.

`date` as TIMESTAMP assumes it's the last-seeded-at. If it's meant to be a date-only value, use `DATE`. Your original stores it as text so the intent isn't clear.

---

## Stops

```typescript

await db.schema

.createTable('stops')

.addColumn('id', 'varchar(36)', col => col.primaryKey())

.addColumn('version', 'integer')

.addColumn('trip_id', 'varchar(36)', col => col.references('trips.id'))

.addColumn('type', 'varchar(30)')

.addColumn('client', 'varchar(200)')

.addColumn('address_id', 'varchar(36)', col => col.references('addresses.id'))

.addColumn('estimated_departure_time', 'timestamp')

.addColumn('estimated_arrival_time', 'timestamp')

.addColumn('note', 'varchar(1000)')

.addColumn('distance_from_previous_stop', 'double')

.addColumn('active', 'smallint', col =>

col.notNull().defaultTo(1).check(sql`active IN (0, 1)`)

)

.addColumn('pre_arrival_radius', 'double')

.addColumn('stop_radius', 'double')

.addColumn('extra_info', 'varchar(1000)')

.addColumn('us_agricultural_exemption_type', 'smallint')

.addColumn('copilot_profile_id', 'integer')

.addColumn('progress_state', 'smallint', col =>

col.check(sql`progress_state IN (0, 1, 2)`)

)

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('finished_at', 'timestamp')

.execute();

```

Note `active` has `defaultTo(1)` matching your original `.default(true)`.

`progress_state` is an enum (`ProgressStateEnum`) stored as integer. SMALLINT with a CHECK for the valid values (`0, 1, 2`). Same for `us_agricultural_exemption_type` — though your original uses `ProgressStateEnum` for that too, which is a bit weird. Copy-paste error in the Drizzle schema? Worth verifying.

---

## Waypoints

```typescript

await db.schema

.createTable('waypoints')

.addColumn('id', 'varchar(36)', col => col.primaryKey())

.addColumn('stop_id', 'varchar(36)', col => col.references('stops.id'))

.addColumn('latitude', 'double')

.addColumn('longitude', 'double')

.execute();

```

No timestamps in your original. Fine.

---

## Tasks

```typescript

await db.schema

.createTable('tasks')

.addColumn('id', 'varchar(36)', col => col.primaryKey())

.addColumn('stop_id', 'varchar(36)', col => col.references('stops.id'))

.addColumn('progress_state', 'smallint', col =>

col.check(sql`progress_state IN (0, 1, 2)`)

)

.addColumn('activity_id', 'integer')

.addColumn('version', 'integer')

.addColumn('name', 'varchar(200)')

.addColumn('active', 'smallint', col => col.check(sql`active IN (0, 1)`))

.addColumn('meta_data_id', 'varchar(36)')

.addColumn('external_validation', 'smallint', col =>

col.check(sql`external_validation IN (0, 1)`)

)

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('finished_at', 'timestamp')

.execute();

```

---

## Trips

```typescript

await db.schema

.createTable('trips')

.addColumn('id', 'varchar(36)', col => col.primaryKey())

.addColumn('version', 'integer')

.addColumn('trip_no', 'varchar(50)')

.addColumn('reference', 'varchar(100)')

.addColumn('note', 'varchar(1000)')

.addColumn('assignation_date', 'timestamp')

.addColumn('distance', 'double')

.addColumn('start_date', 'timestamp')

.addColumn('vehicle_no', 'varchar(36)', col => col.references('vehicles.id'))

.addColumn('active', 'smallint', col =>

col.notNull().defaultTo(1).check(sql`active IN (0, 1)`)

)

.addColumn('copilot_profile_id', 'integer')

.addColumn('sequence_no', 'integer')

.addColumn('progress_state', 'smallint', col =>

col.check(sql`progress_state IN (0, 1, 2)`)

)

.addColumn('driver_no', 'integer', col => col.references('drivers.id'))

.addColumn('finished_at', 'timestamp')

.execute();

```

No `created_at` / `changed_at` in your original. I'm matching your schema, but you may want to add them for consistency.

---

## Vehicles

```typescript

await db.schema

.createTable('vehicles')

.addColumn('id', 'varchar(36)', col => col.primaryKey())

.addColumn('site_id', 'integer', col => col.notNull())

.addColumn('vehicle_state_id', 'integer', col => col.notNull())

.addColumn('vehicle_type_id', 'integer', col => col.notNull())

.addColumn('engine_type_id', 'integer', col => col.notNull())

.addColumn('model_year', 'smallint', col => col.notNull())

.addColumn('description', 'varchar(200)', col => col.notNull())

.addColumn('serial_no', 'varchar(30)', col => col.notNull())

.addColumn('default_operator_no', 'integer')

.addColumn('license_plate', 'varchar(20)', col => col.notNull())

.addColumn('dtc_group_id', 'integer', col => col.notNull())

.addColumn('vehicle_group_id', 'integer', col => col.notNull())

.addColumn('jurisdiction_code', 'varchar(10)', col => col.notNull())

.addColumn('eld_compliant', 'smallint', col =>

col.notNull().check(sql`eld_compliant IN (0, 1)`)

)

.addColumn('emergency_messages_enabled', 'smallint', col =>

col.notNull().check(sql`emergency_messages_enabled IN (0, 1)`)

)

.addColumn('active', 'smallint', col =>

col.notNull().check(sql`active IN (0, 1)`)

)

.addColumn('asset_id', 'varchar(50)', col => col.notNull().unique())

.addColumn('is_out_of_service', 'smallint', col =>

col.notNull().check(sql`is_out_of_service IN (0, 1)`)

)

.addColumn('activity_group_id', 'integer', col => col.notNull())

.addColumn('recorder_installed', 'smallint', col =>

col.notNull().check(sql`recorder_installed IN (0, 1)`)

)

.addColumn('gateway_serial_no', 'integer')

.addColumn('created_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.addColumn('changed_at', 'timestamp', col =>

col.notNull().defaultTo(sql`CURRENT TIMESTAMP`)

)

.execute();

```

`model_year` as SMALLINT — years fit in 2 bytes easily. Minor optimization.

---

## Activities (revised with proper CHECK for enums)

Your original `activities` had `ChangedStateEnum` for `changeState`, and the enum values are (1, 2, 3) not (0, 1, 2). Check your original enum definitions carefully when adding check constraints:

```typescript

.addColumn('change_state', 'smallint', col =>

col.check(sql`change_state IN (1, 2, 3)`) // NEW, UPDATED, DELETED

)

.addColumn('context', 'smallint', col =>

col.check(sql`context IN (0, 1, 2, 4)`) // ZERO, ONE, TWO, FOUR

)

.addColumn('activity_duty_type', 'smallint', col =>

col.check(sql`activity_duty_type IN (0, 1, 2, 3, 4)`) // NONE..SLEEPER

)

```

`ActivityContext` having values `(0, 1, 2, 4)` but skipping 3 is suspicious. If you're sure those are the only valid values, the CHECK is correct. But if the real set is "0-4 but 3 is undefined," you might want `IN (0, 1, 2, 3, 4)` to be safer.

---

## Now: The Helpers — This is Where It Gets Interesting

You shared these SQLite-specific helpers:

```typescript

buildConflictUpdateColumns // SQLite ON CONFLICT DO UPDATE

chunkedInsertOnConflict // batches INSERT+UPDATE

chunkedInsertDoNothing // batches INSERT with ON CONFLICT DO NOTHING

getAddressId // SHA-256 hash for deterministic IDs

```

These won't port to DB2 as-is. Here's what changes:

### SQLite's `ON CONFLICT` → DB2's `MERGE`

SQLite's `INSERT ... ON CONFLICT DO UPDATE SET ...` has no direct DB2 equivalent. DB2 uses `MERGE`:

```sql

MERGE INTO drivers AS target

USING (

VALUES (?, ?, ?, ?)

) AS source (id, first_name, last_name, email)

ON target.id = source.id

WHEN MATCHED THEN UPDATE SET

first_name = source.first_name,

last_name = source.last_name,

email = source.email

WHEN NOT MATCHED THEN INSERT (id, first_name, last_name, email)

VALUES (source.id, source.first_name, source.last_name, source.email)

```

Kysely has a `.mergeInto()` builder that compiles to this. But your "fill_null" and "update-if-not-null" modes are custom logic you'd re-implement in the `WHEN MATCHED` branch:

```sql

WHEN MATCHED THEN UPDATE SET

first_name = CASE WHEN source.first_name IS NOT NULL

THEN source.first_name

ELSE target.first_name END

```

Straightforward, just verbose. The helper rewrite would produce DB2 MERGE statements instead of SQLite UPSERTs. Plan to rewrite these helpers; they're the bulk of the DB2 port effort.

### Chunking: `MAX_INSERT_ROWS = 999` is SQLite-specific

SQLite has a host-variable limit (default 999 parameter markers per statement). DB2 LUW's limit is **32,767 host variables** per statement. You can probably increase `MAX_INSERT_ROWS` significantly — maybe 1000 rows × 30 columns = 30,000 parameters is still under the limit.

But DB2 has a different bottleneck: **statement heap size**. Very large MERGE/INSERT statements can exceed the configured STMT_HEAP parameter. In practice, batches of 500-1000 rows are a sweet spot.

I'd set `MAX_INSERT_ROWS = 500` for DB2 and measure. Adjust based on real performance.

### `getAddressId`: SHA-256 of address components

This is pure application logic, works anywhere. No change needed. It's a deterministic ID derivation — two addresses with the same fields produce the same ID, giving you natural idempotency on inserts.

One bug I noticed though:

```typescript

normalize(address.longitude.toString()), // ← line 7

normalize(address.longitude.toString()), // ← line 8: should be latitude?

```

Two `longitude` calls. The second was probably meant to be `latitude`. Easy to miss but it means your address IDs don't actually depend on latitude. Worth fixing before any data gets hashed with the wrong logic.

---

## Summary

What you have: 15 tables, dozens of boolean columns, many FKs, enum columns, helpers for upserts.

What's already done: you understand the translation patterns (VARCHAR lengths, SMALLINT booleans, TIMESTAMP defaults, identity columns, FKs).

What's ahead:

1. **Write all 15 migration files** in the order shown. This is mostly mechanical now.

2. **Rewrite the helpers** for DB2 MERGE semantics — meaningful work, but isolated.

3. **Fix the `getAddressId` bug** (double-longitude).

4. **Tune `MAX_INSERT_ROWS`** for DB2's host-variable limits.

5. **Handle the `$onUpdateFn` for changed_at** in your app code (or adopt triggers later).

If you want, I can focus the next session on the MERGE-based helper rewrite — it's the most architecturally interesting piece left. Or keep going through migrations. Your call.