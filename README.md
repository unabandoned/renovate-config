# renovate-config

Shared [Renovate](https://docs.renovatebot.com/) presets for the [`@unabandoned`](https://github.com/unabandoned) maintained-fork program.

`@unabandoned/*` packages are upstream-faithful forks of relied-upon-but-abandoned npm packages. These presets keep every fork's own dependency supply chain current, pinned, and automerged from a single shared source of truth, so each fork extends one line instead of copy-pasting config.

## Presets

### `github>unabandoned/renovate-config` (`default.json`)

The base preset every fork should extend. It:

- extends `config:best-practices` — Renovate's recommended baseline (pinned devDependencies, dependency dashboard, and friends);
- pins GitHub Action digests to semver (`helpers:pinGitHubActionDigestsToSemver`) so the CI supply chain is locked to immutable SHAs while staying human-readable;
- disables noisy `@types/node` major bumps (`helpers:disableTypesNodeMajor`);
- surfaces OpenSSF Scorecard data (`security:openssf-scorecard`);
- flags any dependency that itself becomes abandoned (`abandonments:recommended`, >1yr since release) — a monitoring signal, not an auto-action;
- enables vulnerability alerts (`:enableVulnerabilityAlerts`);
- automerges stable non-major updates (`:automergeStableNonMajor`);
- groups linters into a single PR (`group:linters`);
- rebases PRs when they fall behind the base branch, labels them `dependencies`, and runs weekly lock file maintenance.

Usage — add a `renovate.json` at the repo root:

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": ["github>unabandoned/renovate-config"]
}
```

### `github>unabandoned/renovate-config:typescript-eslint-compat` (`typescript-eslint-compat.json`)

Opt-in cap holding TypeScript below 7 until `typescript-eslint`/`typedoc` support the TS7 native compiler ([typescript-eslint#10940](https://github.com/typescript-eslint/typescript-eslint/issues/10940)). Extend it **only** in repos that use `typescript-eslint` or `typedoc`; drop it once they ship TS7 support.

```json
{
  "$schema": "https://docs.renovatebot.com/renovate-schema.json",
  "extends": [
    "github>unabandoned/renovate-config",
    "github>unabandoned/renovate-config:typescript-eslint-compat"
  ]
}
```

## How preset resolution works

`github>unabandoned/renovate-config` resolves to `default.json` at this repo's root; appending `:name` (e.g. `:typescript-eslint-compat`) resolves to `name.json`. This repo is a preset source, not a Renovate-managed repo, so it needs no `renovate.json` of its own.
