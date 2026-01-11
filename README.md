## Infrayantra

---

# infrayantra/pg-fork:17.5.0

Enterprise-grade PostgreSQL 17.5 Docker image with advanced extensions, observability, and automation readiness.

---

## Overview

`infrayantra/pg-fork:17.5.0` is a **curated PostgreSQL 17.5 Docker image** designed for **production workloads**, **DBA automation**, and **advanced observability platforms** such as IntelliDB Enterprise.

This image extends the standard PostgreSQL distribution by bundling **high-value extensions**, providing **clean persistence layout**, and enabling **enterprise diagnostics and tuning workflows** out of the box.

---

## Key Features

- PostgreSQL **17.5** (stable, production-ready)
- Precompiled and bundled extensions
- Optimized for containerized environments
- Supports advanced monitoring and diagnostics
- Suitable for OLTP, analytics, GIS, time-series, and AI workloads
- Compatible with IntelliDB Enterprise tooling

---

## Included PostgreSQL Version

PostgreSQL 17.5

Built for:

* Linux x86_64
* Modern GCC toolchain
* Full PostgreSQL 17 catalog & WAL compatibility

---

## Included Extensions

The following extensions are **pre-installed** and ready to be enabled per database:

| Extension          | Version | Description                      |
| ------------------ | ------- | -------------------------------- |
| pg_stat_statements | 1.11    | Query performance tracking       |
| plpgsql            | 1.0     | Procedural language              |
| postgis            | 3.5.3   | Spatial & GIS support            |
| timescaledb        | 2.23.1  | Time-series workloads            |
| vector             | 0.7.0   | AI / embedding similarity search |

Additional PostgreSQL contrib extensions are also available (e.g. `pgcrypto`, `pg_trgm`, `pageinspect`, `pg_walinspect`, `postgres_fdw`).

---

## Quick Start

### Run Container

```bash
docker run -d \
  --name intellidb-enterprise \
  -p 5555:5555 \
  --restart unless-stopped \
  -v intellidb_data:/var/lib/intellidb/data \
  -v intellidb_logs:/var/log/intellidb \
  -e POSTGRES_USER=intellidb \
  -e POSTGRES_PASSWORD=your_password \
  -e POSTGRES_DB=intellidb \
  -e POSTGRES_PORT=5555 \
  infrayantra/pg-fork:17.5.0
```

---

## Environment Variables

| Variable            | Description            |
| ------------------- | ---------------------- |
| `POSTGRES_USER`     | Database superuser     |
| `POSTGRES_PASSWORD` | Superuser password     |
| `POSTGRES_DB`       | Default database       |
| `POSTGRES_PORT`     | PostgreSQL listen port |

> These variables are applied **only during first initialization**.

---

## Data Persistence

### Directory Layout

| Path                      | Purpose                   |
| ------------------------- | ------------------------- |
| `/var/lib/intellidb/data` | PostgreSQL data directory |
| `/var/log/intellidb`      | PostgreSQL logs           |

### Recommended Volumes

```bash
-v intellidb_data:/var/lib/intellidb/data
-v intellidb_logs:/var/log/intellidb
```

This ensures:

* Data durability
* Safe container recreation
* Separation of data and logs

---

## Enabling Extensions (Per Database)

Extensions are installed system-wide but must be enabled per database.

### Enable pg_stat_statements

```sql
CREATE EXTENSION IF NOT EXISTS pg_stat_statements;
```

### Enable TimescaleDB

```sql
CREATE EXTENSION IF NOT EXISTS timescaledb;
```

### Enable PostGIS

```sql
CREATE EXTENSION IF NOT EXISTS postgis;
CREATE EXTENSION IF NOT EXISTS postgis_topology;
CREATE EXTENSION IF NOT EXISTS postgis_tiger_geocoder;
```

### Enable Vector (AI / embeddings)

```sql
CREATE EXTENSION IF NOT EXISTS vector;
```

Example:

```sql
CREATE TABLE embeddings (
  id BIGSERIAL PRIMARY KEY,
  embedding vector(1536)
);
```

---

## Advanced Diagnostic Extensions

```sql
CREATE EXTENSION IF NOT EXISTS pageinspect;
CREATE EXTENSION IF NOT EXISTS pgstattuple;
CREATE EXTENSION IF NOT EXISTS pg_freespacemap;
CREATE EXTENSION IF NOT EXISTS pg_walinspect;
```

Used for:

* Page-level inspection
* Bloat analysis
* WAL diagnostics
* Storage forensics

---

## shared_preload_libraries

For full observability support, ensure:

```sql
SHOW shared_preload_libraries;
```

Recommended minimum:

```text
pg_stat_statements
```

If required, configure in `postgresql.conf`:

```conf
shared_preload_libraries = 'pg_stat_statements'
```

Then restart the container:

```bash
docker restart intellidb-enterprise
```

---

## Compatibility

This image is compatible with:

* PostgreSQL native clients
* pgBouncer
* Logical & streaming replication
* IntelliDB Enterprise diagnostics & tuning tools
* Monitoring stacks (Prometheus, Grafana via exporters)

---

## Intended Use Cases

* Enterprise OLTP databases
* Time-series ingestion & analytics
* GIS / spatial workloads
* AI vector similarity search
* DBA automation & self-healing systems
* Advanced PostgreSQL diagnostics

---

## Production Recommendations

* Use Docker networks instead of exposing ports publicly
* Add pgBouncer for connection pooling
* Enable WAL archiving for backups
* Monitor using exporters and dashboards
* Use replicas for read scaling and HA

---

## License

PostgreSQL and bundled extensions are licensed under their respective open-source licenses.

---

## Maintained By

**InfraYantra**
Enterprise PostgreSQL Engineering & Automation
