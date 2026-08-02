# Asterinas Unsafe Doc Generator

Extracts all public unsafe APIs from the [Asterinas](https://github.com/asterinas/asterinas)
source code and generates a static HTML report with a searchable/filterable table.

## How it works

1. Clones the Asterinas repository.
2. Recursively scans all Rust files (`.rs`) in the source tree.
3. Analyzes public unsafe items in four categories:
   - `function`: `pub unsafe fn`
   - `method`: `pub unsafe fn` inside `impl`
   - `trait`: `pub unsafe trait`
   - `trait_method`: `unsafe fn` defined inside a `pub unsafe trait` (API name format is `TraitName::MethodName`)
4. Extracts full documentation comments and the `# Safety` section.
5. Generates an HTML page with a table, type filtering, fuzzy search, and JSON export.

## Usage (offline)

```bash
python3 unsafe_doc_generator.py \
  --local-dir "/path/to/asterinas" \
  --remote-repo-url "https://github.com/asterinas/asterinas" \
  --remote-ref "main" \
  --output-dir docs
```

### Arguments

- `--local-dir`: local scan directory (required).
- `--remote-repo-url` + `--remote-ref`: used to generate clickable API links.
- `--remote-path-prefix`: optional manual remote path prefix. If omitted, auto-derived from git root.
- `--output-dir`: output directory (default: `site`).

### Report Fields

| Field | Description |
|-------|-------------|
| Module Path | Derived from the source file path |
| API Name | Function/trait/method name (linked to source) |
| Type | `function`, `method`, `trait`, or `trait_method` |
| Full Doc | Complete doc comment |
| Safety Doc | `# Safety` section only |
| Confirmed | Button with state persisted in browser `localStorage` |

## GitHub Pages

The report is served from the `docs/` folder on the `main` branch:

> **https://safer-rust.github.io/asterinas-unsafe-doc/**

Regenerated automatically by the CI workflow on every push to `main` and
daily at 2 AM UTC.

### Enabling Pages

1. Go to **Settings → Pages**.
2. Under **Source**, select **Deploy from a branch**.
3. Choose branch **`main`** and folder **`/docs`**, then click **Save**.
