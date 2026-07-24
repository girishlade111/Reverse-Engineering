# PROMPT_20.md
## Enterprise Reverse Engineering Prompt Framework — PROMPT_20: Database & Persistence Layer Analysis (Optional)

---

## 1. Prompt Metadata

- **Prompt ID:** PROMPT_20
- **Phase:** 2
- **Stage:** 10 of 10 (Phase 2 capstone, optional)
- **Dependencies:** ART-01 (PROMPT_01), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-11 (PROMPT_11), ART-13 (PROMPT_13).
- **Estimated Tokens:** 11000–17000 (when triggered) / 1500–2500 (when skipped)
- **Output Artifacts:** ART-20 (Doc) — Database & Persistence Report (optional).
- **Author:** EREPF Framework Office
- **Last Revised:** 2026-07-24

---

## 2. Objective

Produce the Database & Persistence Report artifact (ART-20) that identifies the subject repository's persistence model (SQL, NoSQL, KV, document, graph, time-series, file-based), extracts the schema (entities, fields, relations, indexes, constraints), identifies the ORM/ODM (Prisma, TypeORM, SQLAlchemy, Django ORM, Hibernate, Mongoose, etc.), identifies migrations, identifies connection pooling, identifies transactions, and detects read replicas, sharding, and other replication topologies — when triggered by the presence of persistence code; otherwise emit a `SKIPPED` completion record and proceed without producing ART-20.

---

## 3. When to Invoke

PROMPT_20 is OPTIONAL. Its dispatch is governed by the engagement's scope modifier (`MISSION.md` § 6) and by the trigger condition below. As the Phase 2 capstone, PROMPT_20's `status: SUCCESS` (or `SKIPPED` under `SCOPE_FULL`) satisfies the Phase 2 exit conditions and authorizes the orchestrator to begin Phase 3 per `PROJECT_SPECIFICATION.md` § 5.3.

### 3.1 Scope-Modifier Behavior

- **`SCOPE_FULL`** — PROMPT_20 is always dispatched; if the trigger condition does not fire, the prompt emits a `SKIPPED` completion record per § 3.3.
- **`SCOPE_CORE`** — PROMPT_20 is dispatched only if the trigger condition (§ 3.2) fires; if the trigger does not fire, the orchestrator skips PROMPT_20 entirely and records a `NOT_DISPATCHED` note in the engagement manifest.
- **`SCOPE_TRIAGE`** — PROMPT_20 is never dispatched.
- **`SCOPE_MODULE(target)`** — PROMPT_20 is dispatched only if the trigger condition fires within the target module's closure.

### 3.2 Trigger Condition

The trigger condition is satisfied when ANY of the following markers is detected in the in-scope source code or configuration:

- **ORM/ODM imports** — `prisma`, `@prisma/client`, `typeorm`, `sequelize`, `mongoose`, `objection`, `mikro-orm`, `knex`, `sqlalchemy`, `django.db.models`, `SQLModel`, `tortoise-orm`, `peewee`, `Pony`, `asyncpg`, `psycopg2`/`psycopg3`, `pydantic-sqlalchemy`, `sqlmodel`, `Hibernate` (`org.hibernate.*`), `JPA` (`javax.persistence.*`/`jakarta.persistence.*`), `MyBatis`, `jOOQ`, `Ebean`, `ActiveJDBC`, `Spring Data` repositories, `Entity Framework` (`Microsoft.EntityFrameworkCore.*`), `Dapper`, `NHibernate`, `LinqToDb`, `GORM` (Go), `ent` (Go), `sqlx` (Go), `xo`, `pop` (Go), `beego-orm`, `Diesel` (Rust), `SeaORM` (Rust), `SQLx` (Rust), `rusqlite` (Rust), `tokio-postgres` (Rust), `sqlx-postgres`, `ActiveRecord` (Ruby), `Sequel` (Ruby), `Rom` (Ruby), `Ecto` (Elixir), `ArcORM` (Scala), `Slick` (Scala), `Doobie` (Scala).
- **Database-driver imports** — `pg` (Node PostgreSQL), `mysql2`, `mariadb`, `sqlite3`, `better-sqlite3`, `mongodb` (Node driver), `redis`/`ioredis`, `cassandra-driver`, `elasticsearch`, `@opensearch-project/opensearch`, `neo4j-driver`, `influxdb-client`, `dynamodb-sdk`/`@aws-sdk/client-dynamodb`, `@aws-sdk/lib-dynamodb`, `faunadb`.
- **SQL files** — `.sql` files in `migrations/`, `db/migrate/`, `schema/`, `sql/`, `ddl/`, or at the repository root.
- **Migration files** — `prisma/migrations/`, `migrations/` (Alembic), `db/migrate/` (Rails), `flyway/migrations/`, `liquibase/changelog/`, `src/main/resources/db/migration/` (Spring Boot Flyway/Liquibase), `migrations/*.sql` (golang-migrate), `Entity Framework migrations` (`Migrations/*.cs`).
- **Schema-definition files** — `schema.prisma`, `schema.sql`, `schema.rb` (Rails schema dump), `schema.json` (JSON Schema), `database.yml`/`database.yaml` (Rails, Django), `.env` with `DATABASE_URL`.
- **Persistence-marker identifiers** — case-insensitive identifier matches for `repo`, `repository`, `dao`, `mapper`, `entity`, `model`, `table`, `collection`, `migration`, `schema`, `query`, `cursor` in class/variable names combined with persistence-API call patterns.

### 3.3 Skipped Behavior

If the trigger condition does not fire under `SCOPE_FULL`, the prompt emits a `SKIPPED` completion record with the following fields:

```
COMPLETION_RECORD {
  prompt_id: PROMPT_20,
  status: "SKIPPED",
  artifacts_produced: [],
  quality_checks_passed: [],
  quality_checks_failed: [],
  open_questions: [],
  handoff_ready: true,
  notes: "Trigger condition not satisfied; no persistence code detected. ART-20 not produced. Downstream consumers MUST treat ART-20 as ABSENT and not require it for handoff."
}
```

The orchestrator records `ART-20.status: NOT_PRODUCED` in the artifact registry. Downstream prompts that consume ART-20 (PROMPT_11, PROMPT_13, PROMPT_26) MUST degrade gracefully by treating ART-20 as `ABSENT` and recording the degradation in their own Open Questions. Because PROMPT_20 is the Phase 2 capstone, the `SKIPPED` status still satisfies the Phase 2 exit conditions.

---

## 4. Required Inputs

| Input | Type | Consuming Purpose |
|-------|------|-------------------|
| ART-01 | Manifest | In-scope path set; `repository_fingerprint` re-verification per R15. |
| ART-08 | Doc | Class catalog; entity/model classes are extracted from here. Class fields drive schema-field extraction. |
| ART-09 | Doc | Function catalog; repository/DAO functions are detected by name patterns and persistence-API usage. |
| ART-11 | Graph | Data-flow diagrams; flows whose sinks are database writes (per § 6.2 of PROMPT_11) identify persistence boundaries. The significant data types (`D-XX`) that cross persistence boundaries are the entity candidates. |
| ART-13 | Doc | Stateful-unit catalog; units with `kind: persisted` are the persistence candidates. Synchronization mechanisms of kind `transaction` from ART-13 seed transaction-boundary detection (§ 6.6). |
| `OPERATING_RULES.md` | Framework file | Bind R13, R15, R16, R17, R19, R21, R22, R23, R33. |
| `OUTPUT_RULES.md` | Framework file | Conform artifact layout, naming, citation conventions, Mermaid ER-diagram conventions (§ 7). |
| `QUALITY_STANDARDS.md` | Framework file | Apply Doc schema (`§ 4.5`) and type-specific bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4). |

---

## 5. Instructions to AI Agent

1. Evaluate the trigger condition per § 3.2; IF not satisfied AND scope modifier is `SCOPE_FULL`, emit `SKIPPED` completion record per § 3.3 and halt (this still satisfies Phase 2 exit). IF not satisfied AND scope modifier is `SCOPE_CORE` or `SCOPE_MODULE`, halt without emitting a completion record (the orchestrator already skipped dispatch). IF satisfied, continue.
2. Re-verify `repository_fingerprint` matches ART-01; IF not, emit `BLOCKED` with `INTEGRITY_FAIL`.
3. Identify the persistence model per § 6.1 (SQL, NoSQL, KV, document, graph, time-series, file-based, hybrid).
4. Identify the ORM/ODM per § 6.2.
5. Extract the schema per § 6.3 (entities, fields, relations, indexes, constraints).
6. Identify migrations per § 6.4.
7. Analyze connection pooling per § 6.5.
8. Detect transaction boundaries per § 6.6.
9. Infer replication and sharding per § 6.7 (read replicas, sharding strategy, replication topology).
10. Emit Mermaid ER diagrams per § 6.8 with entity-level citations.
11. Cross-check the persistence catalog against ART-13's `kind: persisted` units and ART-11's persistence-boundary flows per § 6.9; unaccounted entities are `CONTRADICTION` findings per R33.
12. Emit ART-20 per § 8 with full front-matter, per-entity sections, migration catalog, transaction catalog, replication catalog, traceability index, open questions.
13. Run the Quality Checks in § 9.
14. Emit the Completion Record per `MASTER_PROMPT.md` § 6; this Completion Record also serves as the Phase 2 exit signal.

---

## 6. Analysis Procedures

### 6.1 Persistence-Model Identification

Identify the persistence model by inspecting database-driver imports and configuration:

- **SQL (relational)** — detected by `pg`, `mysql2`, `mariadb`, `sqlite3`, `better-sqlite3`, `psycopg2`, `asyncpg`, `sqlx` (Rust), `tokio-postgres`, `rusqlite`, `java.sql.*`, `java.jdbc`, `mysql-connector-java`, `postgresql` JDBC driver, `Microsoft.Data.SqlClient`, `Npgsql`, `MySqlConnector`, `System.Data.SQLite`. The specific SQL product is identified from `DATABASE_URL` or connection config: `postgres://` → PostgreSQL, `mysql://` → MySQL, `file:.*\.sqlite` → SQLite.
- **NoSQL (document)** — detected by `mongodb` driver, `@mongodb-js`, `pymongo`, `motor`, `mongo-java-driver`, `mongodb-driver-sync`, `MongoDB.Driver` (.NET), `mongoid` (Ruby), `mongoose` (ODM).
- **NoSQL (column-family)** — detected by `cassandra-driver`, `@astrajs/collections`, `DataStax` Java driver, `gocql`, `scylla-rust-driver`.
- **NoSQL (key-value)** — detected by `redis`/`ioredis`, `redis-py`, `jedis`, `lettuce`, `go-redis`, `redis-rs`, `StackExchange.Redis`, `etcd` client, `memcached` client.
- **NoSQL (graph)** — detected by `neo4j-driver`, `gremlin` clients, `arangodb` drivers, `neptune` clients.
- **Time-series** — detected by `influxdb-client`, `@influxdata/influxdb-client`, `timescaledb` (PostgreSQL extension), `prometheus` remote-write clients, `opentsdb` clients.
- **Search/engine** — detected by `elasticsearch` clients, `@opensearch-project/opensearch`, `meilisearch` clients, `typesense` clients, `algoliasearch`. (Search engines are persistence-adjacent and recorded as `kind: search-engine`.)
- **File-based** — detected by direct file-system persistence: `fs.writeFile` writing JSON/CSV records, `pickle.dump` to files, `Marshal.dump` to files, `readFileSync` of database files (SQLite is also file-based but classified as SQL).
- **Cloud-native managed** — detected by `@aws-sdk/client-dynamodb`, `@aws-sdk/lib-dynamodb`, `aws-sdk-dynamodb` (Java), `boto3.resource('dynamodb')`, AWS SDK S3 (when used for object persistence), Google Datastore/Firestore clients, Azure Cosmos DB clients.
- **Hybrid** — multiple persistence models coexist (e.g., PostgreSQL for relational data, Redis for caching, MongoDB for document logs). Each model is recorded separately.

Each persistence model records `persistence_id` `PS-XX`, `kind`, `product`, `version` (when statically declared), `connection_url_env_var` (when applicable; the URL itself is `REDACTED` per `MISSION.md` § 5), `external: false|true` (for managed services), `citation`.

### 6.2 ORM/ODM Identification

Identify the ORM/ODM:

- **Prisma** — detected by `@prisma/client` imports and `schema.prisma` file. Records `orm_id`, `orm_kind: prisma`, `schema_file`, `client_instantiation_citation`, `generator_config` (from `schema.prisma` `generator` block).
- **TypeORM** — detected by `typeorm` imports, `@Entity()` decorators, `DataSource`/`EntityManager` instantiation. Records `orm_kind: typeorm`, `config_file`, `entities_glob` (the `entities: [...]` glob pattern).
- **Sequelize** — detected by `sequelize` imports, `Model.init()` or `sequelize.define()` calls. Records `orm_kind: sequelize`, `models_glob`.
- **Mongoose** — detected by `mongoose` imports, `mongoose.Schema()`/`mongoose.model()` calls. Records `orm_kind: mongoose`, `models_glob`.
- **SQLAlchemy** — detected by `sqlalchemy` imports, `Base = declarative_base()`, `__tablename__` class attribute. Records `orm_kind: sqlalchemy`, `base_class_citation`, `session_factory_citation`.
- **Django ORM** — detected by `django.db.models.Model` subclasses, `models.py` files in Django apps. Records `orm_kind: django-orm`, `apps_glob`.
- **SQLModel** — detected by `sqlmodel` imports, `class X(SQLModel, table=True)`. Records `orm_kind: sqlmodel`.
- **Peewee** — detected by `peewee.Model` subclasses. Records `orm_kind: peewee`.
- **Tortoise ORM** — detected by `tortoise.models.Model` subclasses. Records `orm_kind: tortoise`.
- **Hibernate / JPA** — detected by `@Entity`, `@Table`, `@Column`, `SessionFactory`, `EntityManager`, `JpaRepository`. Records `orm_kind: hibernate|jpa`, `persistence_xml`, `datasource_config`.
- **MyBatis** — detected by `@Mapper`, `SqlSession`, mapper XML files. Records `orm_kind: mybatis`, `mapper_xml_glob`.
- **jOOQ** — detected by `org.jooq` imports, generated classes. Records `orm_kind: jooq`, `generated_catalog`.
- **Entity Framework (EF Core)** — detected by `Microsoft.EntityFrameworkCore.DbContext` subclasses, `DbSet<T>` properties, `OnModelCreating` overrides. Records `orm_kind: ef-core`, `dbcontext_citation`, `migrations_glob`.
- **Dapper** — detected by `connection.Query<T>(sql)` extensions. Records `orm_kind: dapper` (micro-ORM; schema is extracted from SQL strings).
- **GORM (Go)** — detected by `gorm.DB`, `gorm.Model` embedding, `AutoMigrate(...)`. Records `orm_kind: gorm`, `models_glob`.
- **Ent (Go)** — detected by `entgo.io/ent` imports, generated ent client. Records `orm_kind: ent`, `schema_glob`.
- **SQLx (Rust)** — detected by `sqlx::query`, `sqlx::FromRow`. Records `orm_kind: sqlx` (micro-ORM).
- **Diesel (Rust)** — detected by `diesel::prelude`, `schema.rs`, `#[derive(Queryable)]`. Records `orm_kind: diesel`, `schema_file`.
- **SeaORM (Rust)** — detected by `sea_orm::Entity`, `sea_orm::ActiveModel`. Records `orm_kind: sea-orm`.
- **ActiveRecord (Ruby)** — detected by `ActiveRecord::Base` subclasses, Rails `app/models/*.rb`. Records `orm_kind: activerecord`, `models_glob`.
- **Sequel (Ruby)** — detected by `Sequel::Model` subclasses. Records `orm_kind: sequelp`.
- **Ecto (Elixir)** — detected by `use Ecto.Schema`, `schema "table" do ... end`. Records `orm_kind: ecto`, `schemas_glob`.
- **Slick (Scala)** — detected by `slick.jdbc.PostgresProfile.api._`, `TableQuery`. Records `orm_kind: slick`.
- **Doobie (Scala)** — detected by `doobie` imports, `Fragment.sql`. Records `orm_kind: doobie` (micro-ORM).
- **Raw SQL / driver-only** — no ORM; queries are raw SQL strings. Records `orm_kind: none`, `driver_id` `PS-XX`.

### 6.3 Schema Extraction

Extract the schema by combining model definitions, migration files, and live schema dumps:

- **Entities** — each model class (`@Entity`, `db.Model`, `ActiveRecord::Base`, `Entity` in Prisma schema) is an entity. Each entity records `entity_id` `ENT-XX`, `name`, `table_or_collection_name`, `orm_id`, `class_id` `K-XX` (from ART-08 when in-scope), `file:line-range`.
- **Fields** — each model attribute is a field. Records `field_id` `FLD-XX`, `entity_id`, `name`, `column_name`, `type` (ORM-mapped type + database type), `nullable`, `default`, `unique`, `primary_key`, `foreign_key`, `index`, `citation`.
- **Relations** — detected by ORM relation declarations: `@ManyToOne`, `@OneToMany`, `@ManyToMany`, `@OneToOne` (TypeORM/Hibernate), `ForeignKey`/`relationship()` (SQLAlchemy), `references:` (Rails), `belongsTo`/`hasMany`/`has_and_belongs_to_many` (Mongoose/Sequelize), `relation`/`field` (Prisma), `DbSet<T>` navigation properties (EF Core). Each relation records `relation_id` `REL-XX`, `from_entity` `ENT-XX`, `to_entity` `ENT-XX`, `kind` (one-to-one | one-to-many | many-to-one | many-to-many), `foreign_key_field` `FLD-XX`, `join_table` (for many-to-many), `on_delete` (cascade | restrict | set-null | no-action), `citation`.
- **Indexes** — detected by `@Index` decorators, `index=True` (SQLAlchemy), `add_index` (Rails migrations), `index` blocks in Prisma schema, `CREATE INDEX` in SQL migrations. Each index records `index_id` `IDX-XX`, `entity_id`, `fields` (list), `unique`, `composite`, `citation`.
- **Constraints** — detected by `@Check`, `CHECK` clauses in migrations, `unique_together` (Django), `validates` blocks (Rails), `@Size`/`@Min`/`@Max` (Bean Validation). Each constraint records `constraint_id` `CON-XX`, `entity_id`, `kind` (check | not-null | unique | foreign-key | exclusion), `expression`, `citation`.

### 6.4 Migration Identification

Identify migrations and classify them:

- **Migration files** — every file in a migration directory (per § 3.2) is a migration. Records `migration_id` `MIG-XX`, `tool` (prisma | alembic | rails-activerecord | flyway | liquibase | golang-migrate | ef-core | django-migrations | sqlx-migrate | diesel-migration | custom), `version`/`timestamp`, `name`, `file_path`, `direction` (up | down | both | irreversible), `applied_at` (when recorded in a migrations table — typically not detectable statically), `citation`.
- **Migration content** — parse the migration's body for `CREATE TABLE`, `ALTER TABLE`, `DROP TABLE`, `CREATE INDEX`, `DROP INDEX`, `ADD COLUMN`, `DROP COLUMN`, `ADD CONSTRAINT`, `DROP CONSTRAINT`. Each operation records `op_id` `MOP-XX`, `migration_id`, `kind`, `target_entity`, `details`, `citation`.
- **Migration ordering** — migrations are ordered by version/timestamp. Records `order` within the migration catalog.
- **Schema-vs-migrations drift** — when both a schema file (e.g., `schema.prisma`, `schema.rb`) and migration files exist, cross-check that the schema's current state matches the migrations' cumulative end-state. Drift is a `SCHEMA_MIGRATION_DRIFT` finding.

### 6.5 Connection-Pool Analysis

Analyze connection pooling:

- **Pool configuration** — detected by `pool: { max: 10, min: 2 }` (Sequelize), `maximumPoolSize` (TypeORM/HikariCP), `pool_size` (psycopg2/asyncpg), `SetMaxOpenConns`/`SetMaxIdleConns` (Go `database/sql`), `maxPoolSize` (MongoDB driver), `PoolingOptions` (C# `Npgsql`). Records `pool_id` `POOL-XX`, `persistence_id`, `max_connections`, `min_connections`, `idle_timeout`, `connection_timeout`, `max_lifetime`, `citation`.
- **Pool library** — detected by `HikariCP`, `c3p0`, `Apache DBCP`, `Vibur`, `pgbouncer` (external pooler), `pgcat`, `odyssey`. Records `pool_library`, `external: false|true` (for `pgbouncer`).
- **Pool metrics** — when the codebase exposes pool metrics (e.g., HikariCP `HikariPoolMXBean`), records `metrics_exposed: true|false`.

### 6.6 Transaction-Boundary Detection

Detect transaction boundaries — where transactions begin, commit, and roll back:

- **Declarative transactions** — `@Transactional` (Spring/Java, NestJS), `@Transaction` (Python `databases`), `with transaction.atomic():` (Django), `TransactionScope` (.NET), `@Transactional` (Sequelize), `db.transaction(fn)` (Knex/Objection). Records `txn_id` `TXN-XX`, `kind: declarative`, `boundary_fn` `FN-XX`, `propagation` (REQUIRED | REQUIRES_NEW | NESTED | SUPPORTS | NOT_SUPPORTED | NEVER | MANDATORY), `isolation_level`, `readonly`, `citation`.
- **Programmatic transactions** — `BEGIN`/`COMMIT`/`ROLLBACK` SQL strings, `connection.beginTransaction()`, `tx := db.Begin()` (GORM), `conn.exec("BEGIN")`, `tx.commit()` (SQLAlchemy). Records `txn_id`, `kind: programmatic`, `begin_fn` `FN-XX`, `commit_fn` `FN-XX`, `rollback_fn` `FN-XX`, `citation`.
- **Transaction scope** — the set of `FN-XX` that execute within the transaction. Cross-reference ART-10's call graph: every callee reachable from the boundary function before commit/rollback is in-scope of the transaction.
- **Rollback triggers** — detected by `catch (e) { tx.rollback(); throw e; }` patterns, `except: conn.rollback()`, `defer tx.Rollback()` (Go). Records `rollback_trigger_citation`.
- **Distributed transactions** — two-phase commit (XA), sagas (cross-reference ART-14's event workflows for saga-step boundaries), `JTA` (Java Transaction API), `MSDTC` (Microsoft Distributed Transaction Coordinator). Records `distributed: true`, `coordinator`, `participants`.

Each transaction records `txn_id` `TXN-XX`, `kind`, `boundary_fn`, `scope` (list of `FN-XX`), `isolation_level`, `propagation`, `readonly`, `rollback_triggers`, `distributed`, `citation`.

### 6.7 Replication and Sharding Inference

Infer replication and sharding topology:

- **Read replicas** — detected by separate `DATABASE_URL` env vars (e.g., `REPLICA_DATABASE_URL`), `read_replicas:` config (TypeORM/Sequelize), `ReplicaConnection` (Rails), `ReadReplicaRoutingConnection` (Spring), `UseReadReplica` middleware, `read_preference=secondary` (MongoDB). Each replica records `replica_id` `RPL-XX`, `persistence_id`, `read_write_split_strategy` (manual | automatic-routed | load-balanced), `citation`.
- **Sharding** — detected by `shard_key` config, `ShardedDataSource` (Java), `shard:` config in Prisma/TypeORM, `partition_by` in SQL DDL, `Vitess`/`Citus`/`CockroachDB` drivers, application-level sharding (hash on a key to pick a database connection). Each shard records `shard_id` `SHD-XX`, `persistence_id`, `sharding_strategy` (hash | range | directory | geographic), `shard_key`, `shard_count` (when statically declared), `citation`.
- **Replication topology** — detected by `REPLICAOF` (Redis), `streaming replication` (PostgreSQL config), `MongoDB replica set`, `Cassandra replication_factor`, `Galera cluster`. Records `topology_id` `TOP-XX`, `persistence_id`, `topology_kind` (primary-replica | primary-primary | replica-set | cluster | quorum), `node_count` (when declared), `citation`.

### 6.8 Mermaid ER-Diagram Emission

Emit Mermaid `erDiagram` diagrams per `OUTPUT_RULES.md` § 7:

- **Per-module ER diagram** — one diagram per module (or per bounded context) showing the module's entities and relations. Each entity is rendered as `ENT_XX { type field_name }`. Each relation is rendered as `ENT_XX ||--o{ ENT_YY : "relation_name"`. Diagrams > 30 entities are decomposed by module.
- **Full schema diagram** — a master ER diagram showing every entity and relation. Decomposed into sub-diagrams when > 30 entities; a master index diagram maps modules to sub-diagrams.
- **Migration timeline** — a `graph LR` showing migrations in order with their operations.

Each diagram is preceded by a `**Diagram D-XX: <Title>**` caption and accompanied by a `.mmd` sidecar file.

### 6.9 Coverage Cross-Check

Cross-check the persistence catalog against ART-13's `kind: persisted` units and ART-11's persistence-boundary flows:

1. Compute `E_13` = set of `S-XX` stateful units with `kind: persisted` from ART-13.
2. Compute `E_11` = set of `D-XX` significant data types that cross a persistence boundary in ART-11.
3. Compute `E_20` = set of `ENT-XX` entities in ART-20.
4. Expected: `E_20 ⊇ E_13 ∪ E_11` (every persisted unit and every persistence-boundary-crossing type is an entity in ART-20). Entities in `(E_13 ∪ E_11) \ E_20` are `COVERAGE_GAP` findings.
5. Entities in `E_20 \ (E_13 ∪ E_11)` are entities cataloged in ART-20 that ART-11 and ART-13 do not record as persisted; these are typically lookup-table entities (e.g., country lists, enum-mapping tables) that ART-13 may not classify as stateful and that ART-11 may not trace as flows. These are flagged for review.

---

## 7. Required Outputs

### ART-20 — Database & Persistence Report

**Type:** Doc (optional).

**Acceptance Criteria (when produced):**

- AC-20.1: The artifact file exists at `<output_root>/artifacts/phase2/ART20_<engagement_id>_persistence.md`.
- AC-20.2: The front-matter conforms to `QUALITY_STANDARDS.md` § 4.1 and § 4.5.
- AC-20.3: The body contains: Executive Summary, Methodology, Persistence Models, ORM/ODM Catalog, Schema (entities, fields, relations, indexes, constraints), Migrations, Connection Pools, Transactions, Replication and Sharding, Coverage Cross-Check, Traceability Index, Open Questions, Cross-References.
- AC-20.4: Every entity, field, relation, index, constraint, migration, pool, transaction, and replica cites its source.
- AC-20.5: Every Mermaid ER diagram is preceded by a `**Diagram D-XX: <Title>**` caption.
- AC-20.6: A `.mmd` sidecar file exists for every Mermaid block.
- AC-20.7: Every transaction records its scope, isolation level, and rollback triggers.
- AC-20.8: Coverage cross-check is recorded with no unresolved contradictions.

**Acceptance Criteria (when skipped):**

- AC-20.S1: A `SKIPPED` completion record is emitted with the note from § 3.3.
- AC-20.S2: No artifact file is produced; the artifact registry records `ART-20.status: NOT_PRODUCED`.

---

## 8. Output Templates

### 8.1 ART-20 Front-Matter

```yaml
---
engagement_id: <uuid>
artifact_id: ART-20
artifact_type: Doc
producing_prompt: PROMPT_20
phase: 2
created_at: <ISO-8601>
last_revised: <ISO-8601>
coverage_fraction: <0..1>
quality_score: { coverage: <0..5>, traceability: <0..5>, accuracy: <0..5>, depth: <0..5>, coherence: <0..5>, precision: <0..5>, completeness: <0..5>, readability: <0..5>, aggregate: <0..40> }
status: DRAFT
repository_fingerprint_recheck: <sha-256-hex>
persistence_models:
  - id: PS-01
    kind: sql-relational | nosql-document | nosql-column | nosql-kv | nosql-graph | time-series | search-engine | file-based | cloud-native-managed | hybrid
    product: <name>
    version: <text> | UNVERIFIED
    connection_url_env_var: <name> | NA
    external: false
    citation: <file>:<line-range>
orms:
  - id: ORM-01
    persistence_id: PS-XX
    orm_kind: prisma | typeorm | sequelize | mongoose | sqlalchemy | django-orm | sqlmodel | peewee | tortoise | hibernate | jpa | mybatis | jooq | ef-core | dapper | gorm | ent | sqlx | diesel | sea-orm | activerecord | sequel | ecto | slick | doobie | none
    schema_file: <path> | NA
    client_instantiation_citation: <file>:<line-range>
    config_file: <path> | NA
    entities_glob: <glob> | NA
entities:
  - id: ENT-01
    name: <name>
    table_or_collection_name: <name>
    orm_id: ORM-XX
    class_id: K-XX | NA
    file: <path>
    line_range: <start-end>
fields:
  - id: FLD-01
    entity_id: ENT-XX
    name: <name>
    column_name: <name>
    type: <orm-type> / <db-type>
    nullable: true | false
    default: <value> | none
    unique: true | false
    primary_key: true | false
    foreign_key: true | false
    index: true | false
    citation: <file>:<line-range>
relations:
  - id: REL-01
    from_entity: ENT-XX
    to_entity: ENT-XX
    kind: one-to-one | one-to-many | many-to-one | many-to-many
    foreign_key_field: FLD-XX
    join_table: <name> | NA
    on_delete: cascade | restrict | set-null | no-action | NA
    citation: <file>:<line-range>
indexes:
  - id: IDX-01
    entity_id: ENT-XX
    fields: [<name>]
    unique: true | false
    composite: true | false
    citation: <file>:<line-range>
constraints:
  - id: CON-01
    entity_id: ENT-XX
    kind: check | not-null | unique | foreign-key | exclusion
    expression: <text>
    citation: <file>:<line-range>
migrations:
  - id: MIG-01
    tool: prisma | alembic | rails-activerecord | flyway | liquibase | golang-migrate | ef-core | django-migrations | sqlx-migrate | diesel-migration | custom
    version: <text>
    name: <name>
    file_path: <path>
    direction: up | down | both | irreversible
    operations:
      - op_id: MOP-01
        kind: create-table | alter-table | drop-table | create-index | drop-index | add-column | drop-column | add-constraint | drop-constraint
        target_entity: ENT-XX | NA
        details: <text>
        citation: <file>:<line-range>
    order: <int>
connection_pools:
  - id: POOL-01
    persistence_id: PS-XX
    max_connections: <int>
    min_connections: <int>
    idle_timeout: <text>
    connection_timeout: <text>
    max_lifetime: <text>
    pool_library: <name> | NA
    external: false
    citation: <file>:<line-range>
    metrics_exposed: true | false
transactions:
  - id: TXN-01
    kind: declarative | programmatic
    boundary_fn: FN-XX
    scope: [FN-XX]
    isolation_level: read-uncommitted | read-committed | repeatable-read | serializable | snapshot | NA
    propagation: REQUIRED | REQUIRES_NEW | NESTED | SUPPORTS | NOT_SUPPORTED | NEVER | MANDATORY | NA
    readonly: true | false
    rollback_triggers: [<text>]
    distributed: true | false
    coordinator: <name> | NA
    participants: [<name>] | NA
    citation: <file>:<line-range>
replicas:
  - id: RPL-01
    persistence_id: PS-XX
    read_write_split_strategy: manual | automatic-routed | load-balanced
    citation: <file>:<line-range>
shards:
  - id: SHD-01
    persistence_id: PS-XX
    sharding_strategy: hash | range | directory | geographic
    shard_key: <name>
    shard_count: <int> | UNVERIFIED
    citation: <file>:<line-range>
topologies:
  - id: TOP-01
    persistence_id: PS-XX
    topology_kind: primary-replica | primary-primary | replica-set | cluster | quorum
    node_count: <int> | UNVERIFIED
    citation: <file>:<line-range>
coverage_cross_check:
  entities_from_art13: [S-XX]
  entities_from_art11: [D-XX]
  entities_cataloged: [ENT-XX]
  coverage_gaps: [S-XX | D-XX]
  catalog_only: [ENT-XX]
mermaid_sources:
  - diagram_id: D-01
    title: <text>
    sidecar_file: <relative-path>
    node_count: <int>
source_coverage:
  - path: <file_path>
    symbol_count: <int>
    line_range: <start-end>
open_questions:
  - id: OQ-01
    question: <text>
    blocking: true | false
traceability_index:
  - claim_id: C-01
    source: <file_path>:<line-range>
    symbol: <name>
sections:
  - id: S-01
    title: <string>
    claims: [C-XX]
---
```

### 8.2 ART-20 Body Skeleton

```markdown
# ART-20: Database & Persistence Report

## 1. Executive Summary
## 2. Methodology
## 3. Persistence Models
## 4. ORM/ODM Catalog
## 5. Schema
   ### 5.1 Entities
   **Diagram D-01: ER Diagram (Module: <name>)**
   ```mermaid
   erDiagram
       ENT01[user] {
           string id PK
           string email UK
           string name
       }
       ENT02[post] {
           string id PK
           string author_id FK
           string title
       }
       ENT01 ||--o{ ENT02 : "writes"
   ```
   ### 5.2 Relations
   ### 5.3 Indexes
   ### 5.4 Constraints
## 6. Migrations
   **Diagram D-02: Migration Timeline**
## 7. Connection Pools
## 8. Transactions
## 9. Replication and Sharding
## 10. Coverage Cross-Check
## 11. Traceability Index
## 12. Open Questions
## 13. Cross-References
```

---

## 9. Quality Checks

### Baseline Checks (when produced)

- **Q1. Coverage Check** — every `S-XX` persisted unit from ART-13 and every `D-XX` persistence-boundary type from ART-11 is cataloged as an entity; threshold ≥ 0.90.
- **Q2. Citation Check** — ≥ 0.95 of entities, fields, relations, indexes, constraints, migrations, pools, transactions, replicas, shards, and topologies cited.
- **Q3. Schema Conformance Check** — validates against § 4.5.
- **Q4. Non-Contradiction Check** — no entity in ART-20 contradicts ART-13's `kind: persisted` units or ART-11's persistence-boundary flows.
- **Q5. UNVERIFIED Accounting** — every `UNVERIFIED` product version, shard count, and node count has an Open Question.
- **Q6. Idempotence Spot-Check** — re-running § 6.3 on a 5% sample of entities yields the same field set.
- **Q7. Handoff Readiness** — all Handoff Criteria in § 11 are satisfied.

### Prompt-Specific Checks (when produced)

- **Q-20.A. Schema-Migration Drift** — when both a schema file and migrations exist, drift is recorded as a finding or `no-drift-detected` is recorded with rationale.
- **Q-20.B. Transaction-Scope Completeness** — every `TXN-XX` records its `scope` (list of `FN-XX`); empty scope is non-conformant.
- **Q-20.C. Index Coverage** — every `foreign_key: true` field has a corresponding `IDX-XX` (or an Open Question explaining why no index exists).
- **Q-20.D. Migration Direction Coverage** — every migration's `direction` is `up`, `down`, `both`, or `irreversible`; migrations with only `up` and no `down` are flagged `NO_ROLLBACK` with severity MINOR.
- **Q-20.E. Mermaid Entity Citation** — every entity and relation in the Mermaid ER diagrams has a citation in the accompanying text or the `edge:` comment.
- **Q-20.F. Sidecar Files** — every Mermaid block has a `.mmd` sidecar file.
- **Q-20.G. Connection-Pool Documentation** — every persistence model with a driver-level pool records `max_connections` or `UNVERIFIED` with rationale.

### Skipped Checks (when skipped)

- **Q-20.S1. Trigger Not Satisfied** — the trigger condition (§ 3.2) is re-evaluated; if it now fires, the prompt is re-dispatched. If it does not fire, the `SKIPPED` record is conformant.
- **Q-20.S2. Downstream Degradation** — downstream prompts that consume ART-20 (PROMPT_11, PROMPT_13, PROMPT_26) record the degradation in their Open Questions.

---

## 10. Common Pitfalls

- Do not infer the persistence model from a single ORM import; verify the actual driver and connection configuration per R22.
- Always record the ORM's `entities_glob`; an unspecified glob leaves the entity set unreviewable.
- Do not conflate entities with value objects; entities have identity (primary key), value objects do not.
- Always record the `on_delete` action for foreign-key relations; an unspecified action leaves the referential-integrity model underspecified.
- Do not omit migrations because the codebase uses `AutoMigrate` (GORM) or `synchronize: true` (TypeORM); these auto-migration patterns are recorded as `kind: auto` migrations and flagged with severity MINOR (auto-migration in production is risky).
- Always cross-check the schema against migrations; drift is a real finding, not a stylistic gap.
- Do not infer transactions from `@Transactional` alone; verify the transaction manager configuration and the propagation behavior.
- Always record transaction isolation level; an unspecified isolation level leaves the concurrency model underspecified.
- Do not infer sharding from a `partition_by` clause alone; verify the application's sharding logic (which connection is selected).
- Always cite the connection-pool configuration; an unspecified pool size leaves the scalability model underspecified.
- Do not record `DATABASE_URL` values verbatim; redact per `MISSION.md` § 5 ethical boundaries.
- Always emit `.mmd` sidecar files; PROMPT_25 re-renders ER diagrams from the sidecar source.
- Do not produce ART-20 when the trigger does not fire under `SCOPE_FULL`; the `SKIPPED` record is the conformant output and still satisfies the Phase 2 exit conditions.

---

## 11. Handoff Criteria

PROMPT_11, PROMPT_13, and PROMPT_26 consume ART-20 (when produced). Handoff requires ALL of:

- HC-20.1: ART-20 status is `REVIEWED` or `DRAFT` with orchestrator waiver, OR `SKIPPED` (when trigger does not fire under `SCOPE_FULL`).
- HC-20.2: Every persistence model and ORM/ODM is cataloged.
- HC-20.3: Schema (entities, fields, relations, indexes, constraints) is extracted.
- HC-20.4: Migrations, connection pools, transactions, and replication/sharding topology are cataloged (or `NONE_DETECTED` with rationale).
- HC-20.5: Mermaid ER diagrams are emitted with `.mmd` sidecar files.
- HC-20.6: Coverage cross-check is recorded with no unresolved contradictions.
- HC-20.7: `repository_fingerprint_recheck` matches ART-01.
- HC-20.8: No `BLOCKING` open questions remain.

When ART-20 is `SKIPPED`, HC-20.2 through HC-20.6 are `NA`, and only HC-20.1, HC-20.7, and HC-20.8 apply.

Additionally, as the Phase 2 capstone, PROMPT_20's `status: SUCCESS` or `SKIPPED` (under `SCOPE_FULL`) satisfies the Phase 2 exit conditions per `PROJECT_SPECIFICATION.md` § 5.3 and authorizes the orchestrator to begin Phase 3.

---

## 12. Cross-References

- **Consumed by:** PROMPT_11 (Data Flow — uses ART-20 to identify persistence sinks and entity-loaded sources; degrades gracefully when ART-20 is `SKIPPED`), PROMPT_13 (State Management — uses ART-20 to identify persisted stateful units; degrades gracefully when `SKIPPED`), PROMPT_26 (Rebuild Guide — uses ART-20 as required content for the persistence runbook; degrades gracefully when `SKIPPED`).
- **Depends on:** ART-01 (PROMPT_01), ART-08 (PROMPT_08), ART-09 (PROMPT_09), ART-11 (PROMPT_11), ART-13 (PROMPT_13).
- **Governing rules:** `OPERATING_RULES.md` R13, R15, R16, R17, R19, R21, R22, R23, R33.
- **Schema authority:** `QUALITY_STANDARDS.md` § 4.1, § 4.5; Doc bar (aggregate ≥ 30, Traceability ≥ 4, Depth ≥ 4).
- **Output authority:** `OUTPUT_RULES.md` § 2.4, § 3.1, § 4, § 6, § 7.
- **Forward reference:** PROMPT_30 verifies that every entity referenced by ART-11, ART-13, or ART-26 resolves to an entry in ART-20 (when ART-20 is produced) and that downstream consumers degrade gracefully when ART-20 is `SKIPPED`.
- **Phase boundary:** This prompt's `status: SUCCESS` or `SKIPPED` (under `SCOPE_FULL`) is the Phase 2 exit signal per `PROJECT_SPECIFICATION.md` § 5.3. The orchestrator MUST NOT dispatch PROMPT_21 until HC-20.1 through HC-20.8 are all satisfied.

*End of PROMPT_20. This completes Phase 2 (Dynamics & Behavior). Orchestrator may proceed to Phase 3 (Intelligence & Patterns) upon satisfaction of § 11.*
