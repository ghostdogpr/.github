# Contributing

Thank you for your interest in contributing. These guidelines apply to all of ghostdogpr's Scala libraries, including sage, shard, proteus, purelogic and caliban. Each project's README and documentation site describe how that project is built and tested, along with any other project-specific details.

Everyone taking part in these projects is expected to follow the [Code of Conduct](https://github.com/ghostdogpr/.github/blob/main/CODE_OF_CONDUCT.md).

## Reporting bugs and suggesting features

Open an issue in the project's repository. For a bug, include a clear description of the problem, the steps to reproduce it, and any relevant logs or code. For a feature, describe what you need and, if possible, show some sample code.

Before you start working on a large change, open an issue to discuss it. This lets us check that the change fits the project and that nobody else is already working on it.

## Submitting changes

1. Fork the repository and create a branch for your change.
2. Make your changes.
3. Format the code with `sbt fmt`. Every project defines an sbt `fmt` alias that formats the code with scalafmt, and a `check` alias that verifies the formatting. CI runs `check`, so a pull request with unformatted code fails.
4. Make sure the tests pass. The project's CI configuration lists the exact commands it runs. Some projects, such as sage and shard, run tests against real servers in Docker containers, so Docker must be running to run those tests locally.
5. Push your branch and open a pull request. CI runs on every pull request.

Pull requests are merged with squash merging, so all the commits of a pull request become a single commit on the default branch.
