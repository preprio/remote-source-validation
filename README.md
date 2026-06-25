# Remote Source Validation

Validate JSON files in `remote-source/spec` against the Prepr remote source response schema spec.

## Why use this validator

- Validate remote source response examples before using or sharing them.
- Get file-level error output that is easy to review and fix.
- Keep response examples aligned with the schema in `spec/2026-03-05.json5`.

## Usage

Install dependencies:

```sh
npm install
```

Run validation against the default target:

```sh
npm run validate
```

Run the validator directly with custom paths:

```sh
node scripts/validate-prepr-remote-source.mjs --schema spec/2026-03-05.json5 --target remote-source/spec
```

To write a JSON report, pass `--report-file`:

```sh
node scripts/validate-prepr-remote-source.mjs --schema spec/2026-03-05.json5 --target remote-source/spec --report-file report.json
```

## What gets checked

- Every JSON file under `remote-source/spec` is validated as a remote source endpoint response.
- Validation errors are listed per file.
- Any validation error exits with a non-zero status.
- Missing `remote-source/spec` exits with a non-zero status.
- Empty `remote-source/spec` (no `.json` files) exits with a non-zero status.

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

Place one or more JSON files like this in `remote-source/spec` and run the validator.

## Support

Questions or issues: use [GitHub Issues](../../issues)
