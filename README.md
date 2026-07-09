# CI Test Config

Configuration file for quarantining driver tests in CI.

## Purpose

This repository contains `ci-test-config.json`, which specifies which driver tests should be skipped during CI runs. Use this to temporarily disable flaky or troublesome driver tests while issues are being investigated.

This is kept separate from the main metabase repository so that
changes can be made to a driver at once across all branches without
having to backport to all release branches.

### Fields

- **`drivers`**: Array of drivers to skip tests for. The `name` must match the driver keywords defined in `all-drivers` in [mage/src/mage/modules.clj](https://github.com/metabase/metabase/blob/master/mage/src/mage/modules.clj). The `status` field can be `skip` to allow it to be skipped in PRs which don't change that driver's code, or `info` to have it run without affecting the pass/fail status of the branch.

  Valid driver names as of 2026-02:
  ```
  h2, athena, bigquery, clickhouse, databricks, druid, druid-jdbc,
  mongo, mongo-ssl, mongo-sharded-cluster, mysql-mariadb, oracle,
  postgres, presto-jdbc, redshift, snowflake, sparksql, sqlite,
  sqlserver, vertica
  ```

- **`metadata`**: TODO: what is this for?

- **`ignored.drivers`**: TODO: what is this for? seems redundant with
  top-level `drivers`; is it actually used anywhere?
