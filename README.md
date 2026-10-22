# .github

Organization-wide defaults for `msavarian-base`.

| Path | Purpose |
| --- | --- |
| `profile/README.md` | Public organization profile |
| `.github/workflows/dotnet-ci.yml` | Reusable workflow: restore, build, test, optional pack |
| `.github/workflows/dotnet-nuget-pack.yml` | Reusable workflow: test, pack, check package versions against the tag. The push runs in the caller (see `workflow-templates/dotnet-nuget-publish.yml`) so nuget.org Trusted Publishing can match its file name |
| `.github/workflows/dotnet-github-packages-push.yml` | Reusable workflow: push the `packages` artifact to the organization's GitHub Packages NuGet feed |
| `workflow-templates/` | Starter workflows shown under **Actions → New workflow** |
| `.github/ISSUE_TEMPLATE/`, `.github/pull_request_template.md` | Default issue forms and pull request template |
| `CONTRIBUTING.md`, `SECURITY.md` | Default contribution and security policies |

Repositories that have their own copy of a file use theirs instead of these defaults.

## Using the reusable workflows

```yaml
jobs:
  ci:
    uses: msavarian-base/.github/.github/workflows/dotnet-ci.yml@v1
    with:
      solution: Acme.Orders.slnx
      coverage-threshold: 70   # fail below 70% line coverage (testing standard)
```

See the header comment of each workflow for all inputs.

## Publishing packages

Repositories are private. Every NuGet package is published on a version tag to:

- **nuget.org**, through Trusted Publishing, and then **unlisted by hand** (nuget.org → Manage
  package → Listing); Trusted Publishing keys can only push, and nuget.org cannot push a
  package as unlisted. Unlisted versions still restore when referenced by exact version.
- **GitHub Packages** (`https://nuget.pkg.github.com/msavarian-base/index.json`), through
  `dotnet-github-packages-push.yml`.
- The home-lab **Nexus** feed will be added later.

`workflow-templates/dotnet-nuget-publish.yml` is the full caller.

## Versioning

Callers pin the major tag, `@v1`, never `@main`.

- Compatible changes (new optional inputs, fixes) are merged into `main`, tagged with a full
  version (`v1.2.0`), and the `v1` tag is moved to the same commit:
  `git tag -f v1 && git push -f origin v1`.
- A breaking change (removed or renamed input, changed default behaviour) starts `v2`; `v1`
  stays where it is until callers have moved.
- To try a workflow change before release, point a caller branch at the branch name
  (`@feature/<name>`).

## Roadmap

Planned workflows and changes: [docs/roadmap.md](docs/roadmap.md).
