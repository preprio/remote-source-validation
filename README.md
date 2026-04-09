# Remote Source Validation

GitHub Actions workflow that validates JSON files in `remote-source/spec` against the Prepr remote source response schema spec.

## Why use this GitHub Action

- Validate remote source changes early in pull requests and before merges.
- Enforce consistent spec quality with one workflow across repositories.
- Get file-level error output that is easy to review and fix.
- Prevent invalid remote source updates from reaching `main`.

## Install in your repository

Create `.github/workflows/remote-source-validation.yml` in your repository:

```yaml
name: Validate remote source spec

on:
  workflow_dispatch:
  pull_request:
    paths:
      - 'remote-source/spec/*.json'
  push:
    branches:
      - main
    paths:
      - 'remote-source/spec/*.json'

jobs:
  validate-remote-source:
    uses: preprio/remote-source-validation/.github/workflows/remote-source-validation.yml@v1
```

## What gets checked

- Every JSON file under `remote-source/spec` is validated as a remote source endpoint response.
- Validation errors are listed per file.
- Any validation error fails the job.
- Missing `remote-source/spec` fails the job.
- Empty `remote-source/spec` (no `.json` files) fails the job.

## Remote source response file

The primary example in this repo is a remote source endpoint response:

```json
{
  "filters": [
    {
      "body": "Category",
      "param": "category",
      "display_type": "dropdown",
      "values": {
        "comfort-food": "Comfort food",
        "healthy": "Healthy",
        "seasonal": "Seasonal"
      }
    },
    {
      "body": "Contains nuts",
      "param": "nuts",
      "display_type": "toggle",
      "default_value": false
    }
  ],
  "items": [
    {
      "id": "3177c1b8-a4e2-4a84-85e7-6a02cc0c5f98",
      "body": "Ultimate Vegan Burger",
      "description": "A plant-based burger with a seeded bun and herb mayo.",
      "image_url": "https://images.example.com/products/ultimate-vegan-burger.jpg",
      "external_url": "https://catalog.example.com/products/ultimate-vegan-burger",
      "created_on": "2026-02-12T10:15:30+00:00",
      "changed_on": "2026-03-01T15:48:21+00:00",
      "data": {
        "category": "Comfort food",
        "type": "Main",
        "price": 8.99,
        "nuts": false,
        "cooking_time": 30
      }
    },
    {
      "id": "bd78aeff-1c4a-49da-9cdf-c5ed5bc9c1a2",
      "body": "Green Goddess Bowl",
      "description": "A nourishing bowl with greens, grains, and plant proteins.",
      "image_url": "https://images.example.com/products/green-goddess-bowl.jpg",
      "external_url": "https://catalog.example.com/products/green-goddess-bowl",
      "created_on": "2026-02-15T08:20:15+00:00",
      "changed_on": "2026-03-04T08:25:15+00:00",
      "data": {
        "category": "Healthy",
        "type": "Main",
        "price": 9.5,
        "nuts": false,
        "cooking_time": 25
      }
    }
  ],
  "total": 2
}
```

Place one or more JSON files like this in `remote-source/spec` and the workflow will validate them.

## Workflow outputs

This workflow exposes outputs for downstream jobs:

  - `validation_result` (`success` or `failure`)
  - `files_checked`
  - `invalid_files`
  - `report_json` (JSON string with file-level errors)

Example forwarding to Slack (or any notifier):

```yaml
name: Validate and notify

on:
  pull_request:
    paths:
      - 'remote-source/spec/*.json'

jobs:
  validate:
    uses: preprio/remote-source-validation/.github/workflows/remote-source-validation.yml@v1

  notify:
    runs-on: ubuntu-latest
    needs: validate
    if: always()
    steps:
      - name: Print report
        run: |
          echo "result=${{ needs.validate.outputs.validation_result }}"
          echo "files=${{ needs.validate.outputs.files_checked }}"
          echo "invalid=${{ needs.validate.outputs.invalid_files }}"
          echo '${{ needs.validate.outputs.report_json }}'
```

## Support

Questions or issues: use [GitHub Issues](../../issues)

## Versioning

Use a version tag when referencing the workflow (`@v1`, `@v1.x.y`), not a branch name.
