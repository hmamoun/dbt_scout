# dbt-scout 🕵️‍♂️

**A tactical data warehouse utility package designed to audit warehouse hygiene, hunt down clutter, and scout out unmanaged data assets.**

As analytics environments grow, warehouses naturally collect "data debt"—tables
and views left behind by deprecated models, ad-hoc queries, or structural
changes. `dbt-scout` scans warehouse metadata to surface infrastructure
anomalies and help keep the platform clean, secure, and cost-efficient.

[📦 dbt Hub](https://getdbt.com) | [🐙 GitHub](https://github.com)

---

## Key Features

- **Orphan objects (relations) report:** Lists supported relations in schemas
  used by the dbt project that are not represented in its current dbt graph.

## Orphan Objects (Relations) Report

The `scout_orphan_objects` model lists supported tables and views in schemas
used by the dbt project that are not represented by a model, seed, or snapshot
in its current graph. Each row includes how many queries accessed the relation
in the past 90 days and a suggested command to drop it. The command is provided
for review and is never run automatically. Semantic views are not currently
included.

### Install and use

1. Add the package to your project's `packages.yml`, then run `dbt deps`:

  ```yaml
  packages:
    - git: https://github.com/hmamoun/dbt_scout.git
      revision: main
  ```

2. Add a model to your project that references the report:

  ```sql
  select * from {{ ref('dbt_scout', 'scout_orphan_objects') }}
  ```

When this model is included in `dbt run` (as it is in a default full-project
run), dbt rebuilds the report and the model that selects from it. The report
reflects relations present in the schemas covered by the current dbt graph.
The package requires a configured Snowflake profile with access to the relevant
metadata and usage history.
