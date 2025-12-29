<p align="center">
  <img src="https://app.theneo.io/icons/logo-dark.svg" alt="Theneo Logo" height="60" />
</p>

<h1 align="center">Theneo API Documentation GitHub Action</h1>

<p align="center">
  <strong>Keep your API documentation in sync with your codebase — automatically.</strong>
</p>

<p align="center">
  <a href="https://theneo.io">Website</a> •
  <a href="https://app.theneo.io/theneo/quickstart">Documentation</a> •
  <a href="https://github.com/Theneo-Inc/api-documentation/issues">Issues</a>
</p>

---

## Why Theneo?

Theneo is an AI-powered API documentation platform trusted by top financial organizations, banks and startups worldwide. Unlike traditional documentation tools, Theneo:

- **Generates documentation with AI** — Transform OpenAPI specs into beautiful, Stripe-quality docs instantly
- **Real-time collaboration** — Multiple team members can edit simultaneously
- **Developer portals** — Publish stunning API portals with custom domains and branding
- **Always in sync** — This GitHub Action ensures your docs update automatically with every code change

## Quick Start

### 1. Create your Theneo project

Sign up at [theneo.io](https://theneo.io) and create a documentation project. Import your OpenAPI/Swagger spec or start from scratch.

### 2. Get your API token

Navigate to your [Theneo profile settings](https://app.theneo.io/settings/profile) and copy your API token.

### 3. Add the token to GitHub Secrets

In your GitHub repository, go to **Settings → Secrets and variables → Actions** and create a new secret named `THENEO_SECRET` with your API token.

### 4. Create the workflow file

Add `.github/workflows/theneo.yml` to your repository:

```yaml
name: Update API Documentation

on:
  push:
    branches: [main]
    paths:
      - 'api/**'  # Adjust to match your API spec location

jobs:
  update-docs:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Update Theneo Documentation
        uses: Theneo-Inc/api-documentation@1.8.0
        with:
          FILE_PATH: api/openapi.yaml
          PROJECT_SLUG: your-project-slug
          SECRET: ${{ secrets.THENEO_SECRET }}
```

That's it! Your documentation will now update automatically whenever you push changes.

---

## Finding Your Project Slug

Your project slug is in your Theneo URL:

```
https://app.theneo.io/<workspace_slug>/<project_slug>/<version_slug>
```

For example, if your URL is `https://app.theneo.io/acme/payments-api/v2`, then:
- **Workspace slug:** `acme`
- **Project slug:** `payments-api`
- **Version slug:** `v2`

---

## Configuration Reference

### Required Inputs

| Input | Description |
|-------|-------------|
| `FILE_PATH` | Path to your API specification file (OpenAPI, Swagger, Postman, etc.) |
| `PROJECT_SLUG` | Your project's unique identifier from the Theneo URL |
| `SECRET` | Your Theneo API token (store in GitHub Secrets) |

### Optional Inputs

| Input | Default | Description |
|-------|---------|-------------|
| `WORKSPACE_SLUG` | — | Target a specific workspace |
| `VERSION_SLUG` | — | Target a specific version (uses default version if not set) |
| `IMPORT_OPTION` | `overwrite` | How to handle the import: `overwrite`, `merge`, `endpoints`, or `append` |
| `AUTO_PUBLISH` | `false` | Automatically publish after import |
| `INCLUDE_GITHUB_METADATA` | `false` | Include GitHub actor info in Theneo editor |
| `SECTION_DESCRIPTION_MERGE_STRATEGY` | — | `keep_new` or `keep_old` for section descriptions |
| `PARAMETER_DESCRIPTION_MERGE_STRATEGY` | — | `keep_new` or `keep_old` for parameter descriptions |

---

## Import Options Explained

| Option | Use Case |
|--------|----------|
| **`overwrite`** | Complete replacement — best for spec-first workflows where the OpenAPI file is the source of truth |
| **`merge`** | Attempts to merge changes while preserving manual edits in Theneo |
| **`endpoints`** | Adds new endpoints to a dedicated section for manual organization |
| **`append`** | Adds new content without modifying existing documentation |

---

## Examples

### Auto-publish on merge to main

```yaml
name: Publish API Docs

on:
  push:
    branches: [main]

jobs:
  publish:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Publish to Theneo
        uses: Theneo-Inc/api-documentation@1.8.0
        with:
          FILE_PATH: docs/openapi.yaml
          PROJECT_SLUG: my-api
          SECRET: ${{ secrets.THENEO_SECRET }}
          AUTO_PUBLISH: true
          IMPORT_OPTION: overwrite
```

### Preview on pull request (no auto-publish)

```yaml
name: Preview API Docs

on:
  pull_request:
    branches: [main]

jobs:
  preview:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Preview in Theneo
        uses: Theneo-Inc/api-documentation@1.8.0
        with:
          FILE_PATH: docs/openapi.yaml
          PROJECT_SLUG: my-api
          SECRET: ${{ secrets.THENEO_SECRET }}
          AUTO_PUBLISH: false
          INCLUDE_GITHUB_METADATA: true
```

### Preserve manual edits with merge strategy

```yaml
- name: Update with preserved descriptions
  uses: Theneo-Inc/api-documentation@1.8.0
  with:
    FILE_PATH: api/spec.yaml
    PROJECT_SLUG: my-api
    WORKSPACE_SLUG: my-workspace
    VERSION_SLUG: v2
    SECRET: ${{ secrets.THENEO_SECRET }}
    IMPORT_OPTION: merge
    SECTION_DESCRIPTION_MERGE_STRATEGY: keep_old
    PARAMETER_DESCRIPTION_MERGE_STRATEGY: keep_new
    AUTO_PUBLISH: true
```

---

## Migrating from Deprecated Inputs

| Old Input | New Input |
|-----------|-----------|
| `PROJECT_KEY` | `PROJECT_SLUG` |
| `PATH` | `FILE_PATH` |

---

## Support

- 📖 [Full Documentation](https://app.theneo.io/theneo/quickstart)
- 💬 [Contact Support](mailto:hello@theneo.io)
- 🐛 [Report Issues](https://github.com/Theneo-Inc/api-documentation/issues)

---

## License

MIT License — see [LICENSE](LICENSE) for details.
