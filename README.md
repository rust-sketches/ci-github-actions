# ci-github-actions

This repo codifies some "best practices" for software development in general.

The language of choice here happens to be Rust, but these patterns could be applied to code in any language(s).

## the `main` branch should be protected

This is configured [here](https://github.com/rust-sketches/ci-github-actions/settings/branches) (you won't be able to see this page unless you are a maintainer of this repo).

- no `push`ing directly to `main`
- all code changes require a pull request (for traceability)
- all pull requests require at least 1 approval (for oversight)
- no bypassing of the above rules

## a CI pipeline should run on each PR

This is defined in [pull-request.yml](.github/workflows/pull-request.yml).

Most CI pipelines should

- check formatting / lints
- check documentation coverage (if possible)
- check that the code compiles
- run unit and integration tests (and, usually simultaneously, check test coverage)

We put the more computationally-expensive tasks later, so that if the formatting check fails, we haven't already wasted a bunch of CPU cycles compiling and running the tests.

## we should be able to reproduce CI failures locally

The easiest way to do this is to run _the same CI pipeline_ locally and remotely.

This MR defines its CI pipeline in a `pre-push` Git hook, and the CI pipeline simply runs this hook directly.

In order to keep Git hooks under source control, they must be defined outside the `.git` directory. This repo follows the convention of defining a `.githooks` directory and placing hooks there. To configure Git to automatically run these hooks in response to Git events, you must change the default hook directory with

```shell
git config --local core.hooksPath .githooks
```

You also need to make all hooks executable with

```shell
git update-index --chmod=+x .githooks/*
```

We use `git` above, instead of `chmod` directly, for cross-platform compatibility.

To run (the bulk of) the CI pipeline locally, simply execute the `pre-push` hook as a script

```shell
sh .githooks/pre-push
```

See: https://stackoverflow.com/a/40979016

## see also

- https://docs.github.com/en/actions/learn-github-actions
- https://github.com/actions/starter-workflows/blob/main/ci/rust.yml
- https://github.blog/changelog/2020-07-22-github-actions-better-support-for-alternative-default-branch-names/
- https://github.com/rust-sketches/cd-github-actions (creates the Docker image used by this CI pipeline)