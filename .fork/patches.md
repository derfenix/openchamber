# Fork patches

Changes that live on top of `upstream/main` on this `fork` branch. The scheduled
Upstream Sync task reads the list below, checks every entry against `main`, and
drops the ones that have landed upstream. `fork` is rebuilt as `main` plus the
entries still listed here, so this file is the only source of truth for what is
ahead of upstream. Nothing else may be committed to `fork`.

```yaml
active:
  - type: local
    branch: local/devenv
    note: keep the local devenv setup out of git; never retire this one
```

## Entry format

```yaml
active:
  - type: local
    branch: local/<slug>
    note: <fork-only, never sent upstream; never retire>
  - type: feat
    branch: feat/<slug>
    pr: https://github.com/openchamber/openchamber/pull/<number>
  - type: cherry-pick
    commits: [<sha>, <sha>]
    note: <why it is here and when to drop it>
```

- `local` — fork-only change that will not go upstream. Never retired.
- `feat` — an own change sent to upstream as a PR. Drop it once
  `git log main --grep "(#<number>)"` finds the squash commit.
- `cherry-pick` — an upstream commit taken before its release. Drop it once
  `git cherry main` reports it as applied.

Rebuild `fork` as `main` plus these entries; this file itself is not a patch:
never list it and never drop it.
