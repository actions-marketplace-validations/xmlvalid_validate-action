# xmlvalid GitHub Action

Validates XML files in your repository against XSD 1.0, DTD or RelaxNG schemas with
[xmlvalid.com](https://xmlvalid.com), and marks the failing lines in the pull request.

```yaml
name: XML
on: [push, pull_request]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: xmlvalid/validate-action@v1
        with:
          files: |
            invoices/**/*.xml
          schema: ubl-2.1-invoice
          api-key: ${{ secrets.XMLVALID_API_KEY }}
```

Make a key under [Dashboard → API keys](https://xmlvalid.com/dashboard/api) and store it
as the `XMLVALID_API_KEY` secret. Each file is one API validation (Free: 100 a month,
Pro: 10,000).

| Input | |
|---|---|
| `files` | files or globs, one per line or space-separated |
| `urls` | public URLs, one per line |
| `schema` | a catalog slug (`atom-1.0`) or a saved schema (`my-12`) |
| `schema-file` | an XSD, DTD or RelaxNG file in the repository |
| `rules` | your own Schematron (`.sch`) file in the repository, run after the schema check |
| `doctype` | `true` to validate against each document's DOCTYPE |
| `fail-on-warnings` | `true` to fail on warnings |
| `api-key` | required |
| `api-url` | for a self-hosted install |

A schema that spans several files (imports, includes) is easiest to save once in the
dashboard and use by its `my-…` name.

The step runs [`@xmlvalid/cli`](https://www.npmjs.com/package/@xmlvalid/cli) with
`npx`; the runner needs Node.js 20 or newer (GitHub-hosted runners have it).
