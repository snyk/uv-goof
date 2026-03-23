# workspace-mixed-sources

A uv workspace designed to produce every `source` type that can appear in a
`uv.lock` file, each with transitive dependencies that flow through to
`uv export`.

## Source types in this example

| Source type | Description |
|---|---|
| `virtual` | Project with no `[build-system]` |
| `editable` | Workspace members (editable by default) |
| `directory` | Local path dependencies (also workspace members with `editable = false`) |
| `registry` | Standard PyPI dependencies |
| `git` | Git dependency pinned to a tag |
| `path` | Local wheel (`.whl`) or source distribution archive |
| `url` | URL dependency pinned to a wheel |

A local path dependency (`directory`) can live inside or outside the workspace tree.

## pyproject.toml sources

Each source type is declared in `[tool.uv.sources]` within the relevant `pyproject.toml`.

- `virtual` and `registry` sources require no source entry
- `virtual` is implicit when `[build-system]` is omitted
- `registry` is the default for any dependency without a source override.

```toml
[tool.uv.sources]

# workspace member (editable)
member-a = { workspace = true }

# workspace member - non-editable (directory)
member-b = { workspace = true, editable = false }

# local path dependency (directory)
external-lib = { path = "../../external-lib" }

# git dependency
markdown-it-py = { git = "https://github.com/...", tag = "v3.0.0" }

# local wheel (path)
python-dateutil = { path = "../vendor/python_dateutil-...whl" }

# url dependency
beautifulsoup4 = { url = "https://files.pythonhosted.org/..." }
```
