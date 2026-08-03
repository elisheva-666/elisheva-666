# Case Study: Semantic Layer Platform

*A production data platform built at my current role. The codebase itself is private/proprietary, so this write-up describes the architecture and design decisions rather than the implementation.*

## The Problem

Teams needed to run real-time, large-scale analytical queries (sums, averages, counts, and more complex aggregations) across data that lives in different systems — primarily **BigQuery**, but also **SQL Server**, **PostgreSQL** and **SQLite** — without hand-writing a new SQL query for every report, and without locking the platform to a single database engine.

## Architecture

**1. JSON-based metadata engine.** Instead of hard-coding queries, the platform describes *what* data to fetch and *how* to aggregate it as metadata (entities, fields, relationships, aggregation type). A query-composition layer turns that metadata into the actual SQL sent to the database at request time.

**2. Database-agnostic core.** The engine doesn't assume a specific database. Using **reflection**, entities are mapped to their underlying tables/columns at runtime, so the same metadata-driven query logic can target BigQuery in production while generically supporting SQL Server, PostgreSQL or SQLite — new data sources can be added without rewriting the query engine itself.

**3. Multi-source integration.** Beyond a single database, the platform integrates and combines data coming from multiple sources, backed by hand-written complex SQL procedures where the generic engine isn't enough.

## Why it's interesting

The core challenge wasn't just "write SQL" — it was designing a metadata model expressive enough to describe arbitrary aggregations declaratively, and a runtime layer that can translate that model into correct, efficient SQL for whichever database is behind it, without the caller needing to know which engine that is.

## Tech Stack

Python · SQL · BigQuery · SQL Server · PostgreSQL · SQLite · ASP.NET Core · Angular · EF Core · TypeScript
