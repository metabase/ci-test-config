# CI Test Configuration

This repository contains configuration for skipping flaky or problematic tests in the Metabase CI environment.

## How It Works

The `ci-test-config.json` file is fetched from the master branch during CI runs (in the `prepare-backend` action) and used to skip specific tests that are known to be flaky or problematic.

```yaml
- name: Fetch CI config
  shell: bash
  run: |
    curl -o ci-test-config.json https://raw.githubusercontent.com/metabase/metabase/refs/heads/master/ci-test-config.json
```

## Configuration Structure

The config is a simple array of test skip entries. Each entry contains one or both:

- `clojure_vars` (optional) - Array of fully-qualified Clojure test var names to skip
- `cypress_tests` (optional) - Array of Cypress test objects with `describe` and `it` fields (for use with cypress-grep)

And optionally 

- `_comment` (optional) - Explanation of why these tests are being skipped

### Example

```json
{
  "version": "1.0.0",
  "skipped_tests": [
    {
      "_comment": "send-email test has schema error if emails from another thread are present, sometimes",
      "clojure_vars": [
        "metabase.session.models.session-test/send-email-on-first-login-from-new-device-test"
      ]
    },
    {
      "_comment": "Flaky dashboard filter interactions",
      "cypress_tests": [
        {
          "describe": "Dashboard Filters",
          "it": "should handle date ranges"
        }
      ]
    }
  ]
}
```

## Adding a Test Skip

1. Add a new entry to the `skipped_tests` array
2. Include a clear `_comment` explaining why the test is being skipped
3. Add the appropriate test identifiers:
   - For Clojure: fully-qualified var names in `clojure_vars`
   - For Cypress: objects with `describe` and `it` strings in `cypress_tests`
4. Commit and push to master
5. Git history will track who made the change and when

## Removing a Test Skip

When a test is fixed:
1. Remove the entry from the `skipped_tests` array
2. Commit with a message explaining the fix
3. Push to master

## Notes

- Optionally include a clear `_comment`, and/or write good a commit message - we need to understand why a test was skipped
- Use git blame/log to track who added a skip and when
- This config is fetched from master in the [metabase repo](https://github.com/metabase/metabase/blob/read-ci-test-config-from-the-new-repo/.github/actions/prepare-backend/action.yml#L34), so changes take effect immediately for all CI runs
- Remove skips when tests are fixed
