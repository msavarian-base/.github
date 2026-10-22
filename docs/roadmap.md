# Roadmap — msavarian-base/.github

Organization-wide defaults and **reusable GitHub Actions workflows** that every repository
calls instead of copying pipeline steps ([CI/CD][r-cicd]). The other organizations' `.github`
repositories keep their own profile and templates and hold their own copy of these workflows
(G3), synced from here.

Shared standards: <https://github.com/msavarian/ai-agent-standards> (cloned at
`D:\sources\github.com\.claude`). Read the linked rule before starting an item.

## How to work through this roadmap

- One item per branch, merged into `main` ([git][r-git]).
- Every workflow documents its inputs, secrets and an example caller in its header comment
  and in the README table.
- Test a workflow change from a real caller repository (on a branch) before merging.
- Workflows are versioned: callers pin `@v1` (tag) once item 0.1 is done, not `@main`.

## Decisions to confirm

| # | Decision | Options (recommended in bold) | Status |
|---|---|---|---|
| G1 | How callers pin workflows | **`@v1` major tag, moved on compatible changes** / `@main` | Decided: `@v1` |
| G2 | Deployment mechanism for Docker Compose targets | **SSH to the server + `docker compose pull && up -d`** / a self-hosted runner on the server | Decided: self-hosted runner |
| G3 | Where the other orgs' callers point | **all call `msavarian-base/.github` workflows** / each org copies them | Decided: each org copies them |

## Phase 0 — Housekeeping

- [x] **0.1 Versioning** — tag the current workflows `v1`; document pinning (G1).
- [ ] **0.2 PR template checklist** — add the standards' key checks: Conventional Commits
  title, branch merged into `main`, tests and coverage, README/CHANGELOG, no secrets except
  in `appsettings.*.json` with `TODO(vault)` ([configuration and secrets][r-config]).
- [ ] **0.3 Profile** — `profile/README.md` lists the framework, templates and central
  services with one line each.

## Phase 1 — .NET workflows

- [ ] **1.1 `dotnet-ci.yml`** — add coverage collection and a **70% threshold** input
  ([testing][r-testing]); Testcontainers support (Docker on the runner); NuGet audit.
  - [x] `coverage-threshold` and `coverage-assembly-filters` inputs (branch
    `feature/ci-coverage-threshold`; still to be tested from `dotnet-framework` before merging).
- [ ] **1.2 `dotnet-nuget-pack.yml`** — keep; add a check that the tag commit is on `main`
  ([git][r-git]).
- [ ] **1.3 `dotnet-template-pack.yml`** — pack `dotnet new` templates, install them, generate
  a solution and run its tests before publishing.
- [ ] **1.4 `dotnet-migrations-bundle.yml`** — build an EF Core migration bundle as an artifact
  for the deploy step ([deployment][r-deploy]).

## Phase 2 — Containers and deployment

- [ ] **2.1 `docker-build-push.yml`** — multi-arch build, push to **GHCR** by default or the
  home-lab **Nexus** registry by input; tags = version + commit SHA ([deployment][r-deploy]).
- [ ] **2.2 `deploy-compose.yml`** — deploy a tagged image to a Docker Compose host (G2) per
  environment (`staging`, `production`), run the migration bundle first, health-check after.
- [ ] **2.3 `deploy-k8s.yml`** — apply Kubernetes manifests / Helm chart with the new tag.
- [ ] **2.4 Rollback** — re-deploy a previous tag through `workflow_dispatch`.
- [ ] **2.5 Release guard** — deploy and publish only from tags on `main`.

## Phase 3 — Angular workflows

- [ ] **3.1 `angular-ci.yml`** — install, lint, unit tests with coverage, build (SPA and SSR).
- [ ] **3.2 `angular-e2e.yml`** — Playwright end-to-end tests against a preview build.
- [ ] **3.3** Container build and deploy reuse Phase 2.

## Phase 4 — Quality and security

- [ ] **4.1 PR title check** — Conventional Commits ([git][r-git]).
- [ ] **4.2 Dependabot** starter config (NuGet, npm, GitHub Actions, Docker).
- [ ] **4.3 Vulnerable-package scan** — `dotnet list package --vulnerable`, `npm audit`
  ([security][r-security]).
- [ ] **4.4 Starter workflows** in `workflow-templates/` for every workflow above.

## Phase 5 — Other organizations

- [ ] **5.1** Each org's `.github`: profile, issue forms and PR template aligned with this one,
  plus a copy of these workflows at the same tag; callers reference their own org's copy (G3).

[r-cicd]: https://github.com/msavarian/ai-agent-standards/blob/main/rules/ci-cd.md
[r-git]: https://github.com/msavarian/ai-agent-standards/blob/main/rules/git.md
[r-config]: https://github.com/msavarian/ai-agent-standards/blob/main/rules/configuration-secrets.md
[r-testing]: https://github.com/msavarian/ai-agent-standards/blob/main/rules/testing.md
[r-deploy]: https://github.com/msavarian/ai-agent-standards/blob/main/rules/deployment.md
[r-security]: https://github.com/msavarian/ai-agent-standards/blob/main/rules/security.md
