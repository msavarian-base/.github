# .github

Organization-wide defaults for `msavarian-base`.

| Path | Purpose |
| --- | --- |
| `profile/README.md` | Public organization profile |
| `.github/workflows/dotnet-ci.yml` | Reusable workflow: restore, build, test, optional pack |
| `.github/workflows/dotnet-nuget-pack.yml` | Reusable workflow: test, pack, check package versions against the tag. The push runs in the caller (see `workflow-templates/dotnet-nuget-publish.yml`) so nuget.org Trusted Publishing can match its file name |
| `workflow-templates/` | Starter workflows shown under **Actions → New workflow** |
| `.github/ISSUE_TEMPLATE/`, `.github/pull_request_template.md` | Default issue forms and pull request template |
| `CONTRIBUTING.md`, `SECURITY.md` | Default contribution and security policies |

Repositories that have their own copy of a file use theirs instead of these defaults.

## Using the reusable workflows

```yaml
jobs:
  ci:
    uses: msavarian-base/.github/.github/workflows/dotnet-ci.yml@main
    with:
      solution: Acme.Orders.slnx
```

See the header comment of each workflow for all inputs.
