# Arcade Agent — Architecture Drift (GitHub Action)

> One PR comment, deterministic: what changed in your architecture, not just your lines.

Powered by [arcade-agent](https://github.com/arcade-agent/arcade-agent). MIT.

## What you get

| On | You see |
|---|---|
| Every PR | A single updated comment: components added/removed/renamed, new or resolved smells, metric deltas (entities, edges, RCI) |
| Every merge to default branch | A fresh baseline artifact, versioned, so the next PR compares against real main |

One comment per PR, updated in place — never spam. Fork PRs are skipped (GitHub gives no write token there). Informational by default: no merge threshold imposed.

## Quick start (2 minutes, 0 dependencies)

```yaml
# .github/workflows/arch-drift.yml
name: Architecture drift
on:
  pull_request:
    types: [opened, synchronize, reopened]
  push:
    branches: [main]

jobs:
  drift:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: actions/checkout@v4
      - uses: tuannx/arcade-agent-action@v1
        with:
          language: java   # or python, typescript, c, go, kotlin, rust, multi — blank = auto
```

That's it. The action installs released `arcade-agent` from PyPI (no repo checkout of arcade-agent itself), runs the analysis, uploads the HTML report as an artifact, and posts the comment.

## Inputs

| Input | Default | Meaning |
|---|---|---|
| `arcade-agent-version` | `0.4.2` | Released arcade-agent version to install |
| `python-version` | `3.12` | Python for the analysis runner |
| `source-path` | `.` | Project root, relative to workspace |
| `language` | `` | Language override; blank = auto-detect |
| `exclude-tests` | `true` | Built-in test/vendor/build exclusion policy |

## Positioning

Your PR diff tells reviewers *what* changed. This tells them *whether the architecture moved*: a new cross-component edge, a dependency cycle, a component that doubled in size. Deterministic — same code, same report, every run. History lives in your repo's artifacts, not in a SaaS.

## Marketplace

Published on the GitHub Marketplace under **Code quality**. Maintained as a thin, version-pinned wrapper over [`arcade-agent/arcade-agent/actions/analyze`](https://github.com/arcade-agent/arcade-agent/tree/main/actions/analyze); all analysis logic lives upstream.
