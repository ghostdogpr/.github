# ghostdogpr/.github

This repository holds files shared by ghostdogpr's Scala libraries, such as sage, shard, proteus, purelogic and caliban.

## Default community files

GitHub uses the files at the root of this repository for every ghostdogpr repository that does not have its own copy:

- `CODE_OF_CONDUCT.md` contains the Code of Conduct, which is based on the Scala Code of Conduct.
- `CONTRIBUTING.md` explains how to report issues and submit pull requests.

## Setup action

The `actions/setup` composite action prepares a job to run sbt. It checks out the repository, restores the Coursier cache, installs a Temurin JDK with `actions/setup-java`, and installs sbt with `sbt/setup-sbt`. The action performs the checkout itself, so a job should not check out the repository before using it.

```yaml
jobs:
  test:
    runs-on: ubuntu-24.04
    steps:
      - uses: ghostdogpr/.github/actions/setup@v1
      - run: sbt test
```

The action accepts these inputs:

| Input          | Default | Description                                                                         |
| -------------- | ------- | ----------------------------------------------------------------------------------- |
| `java-version` | `25`    | The Temurin JDK version to install.                                                 |
| `fetch-depth`  | `0`     | The number of commits to fetch. The default fetches the full history and all tags. |

## Docs workflow

The `.github/workflows/docs.yml` reusable workflow builds a VitePress site from the `docs` directory and deploys it to GitHub Pages. Before the build, it replaces the `@VERSION@` placeholder in the Markdown files with the released version. On a `release` event, it uses the tag of that release. On any other event, it uses the latest release that is not a draft, including pre-releases. On a pull request, the workflow builds the site but does not deploy it.

The calling workflow defines the triggers and the concurrency group, and it must grant the `contents: read`, `pages: write` and `id-token: write` permissions. It does not need to pass any secrets.

```yaml
name: Docs

on:
  release:
    types: [published]
  pull_request:
    paths:
      - 'docs/**'
      - '.github/workflows/docs.yml'
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: ${{ github.event_name == 'pull_request' && format('docs-pr-{0}', github.ref) || 'pages' }}
  cancel-in-progress: ${{ github.event_name == 'pull_request' }}

jobs:
  docs:
    uses: ghostdogpr/.github/.github/workflows/docs.yml@v1
```

The site must be in a `docs` directory with a `package-lock.json` file and a `docs:build` npm script that writes the site to `docs/.vitepress/dist`.

## Agent instructions

The `agents/shared.md` file holds the sections shared by the `AGENTS.md` files of sage, shard, proteus, purelogic and caliban. Each library's `AGENTS.md` starts with a section specific to that repository, followed by a copy of this file. To change a shared section, edit `agents/shared.md` here and copy the change to each library.
