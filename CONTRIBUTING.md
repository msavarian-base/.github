# Contributing

Repositories in `msavarian-base` are private and maintained by members of the organization.

## Workflow

1. Create a branch from `main`: `feature/<short-name>`, `fix/<short-name>` or `chore/<short-name>`.
2. Keep commits small, with a message in the imperative mood ("Add RabbitMQ transport").
3. Open a pull request and fill in the template. CI must pass before merging.
4. Squash-merge into `main`.

## .NET repositories

- SDK version comes from `global.json`; shared build settings from `Directory.Build.props`.
- Package versions live only in `Directory.Packages.props` (Central Package Management).
- Follow `.editorconfig`. `dotnet build` should have no new warnings and `dotnet test` must pass.
- Do not add packages with commercial or source-available licenses without a recorded decision.
- Update `CHANGELOG.md` and the docs that describe what you changed.
- Each repository's `AGENTS.md` has the rules for AI coding agents and is a good summary for people too.

## Releases

Packages are published by pushing a version tag (`v10.0.1`) on `main`; see the repository README.
