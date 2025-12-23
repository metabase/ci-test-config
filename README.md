# CI Test Config

Configuration file for quarantining driver tests in CI.

## Purpose

This repository contains `ci-test-config.json`, which specifies which driver tests should be skipped during CI runs. Use this to temporarily disable flaky or troublesome driver tests while issues are being investigated.

### Fields

- **`ignored.drivers`**: Array of driver names to skip tests for. Names must match exactly what appears in the `DRIVERS:` lines in [metabase/.github/workflows/drivers.yml](https://github.com/metabase/metabase/blob/master/.github/workflows/drivers.yml)

  Valid driver names as of 2025-12:
  ```
  athena, bigquery-cloud-sdk, clickhouse, databricks, druid, druid-jdbc,
  mongo, mysql, oracle, postgres, presto-jdbc, redshift, snowflake,
  sparksql, sqlite, sqlserver, vertica
  ```

- **`metadata`**: Tracking information for the changes
  - `last_updated`: Date of the change
  - `updated_by`: Who made the change
  - `reason`: Why the tests are being quarantined

## Usage

To quarantine a driver's tests:

1. Add the driver name to `ignored.drivers`
2. Update the `metadata` section with date, your name, and reason
3. Open a PR with your changes

To restore tests after the issue is fixed:

1. Remove the driver from `ignored.drivers`
2. Update `metadata` accordingly
