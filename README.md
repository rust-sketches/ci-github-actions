# ci-github-actions

see

- https://docs.github.com/en/actions/learn-github-actions
- https://github.com/actions/starter-workflows/blob/main/ci/rust.yml
- https://github.blog/changelog/2020-07-22-github-actions-better-support-for-alternative-default-branch-names/

also [branch protection](https://github.com/rust-sketches/ci-github-actions/settings/branches)

- no `push`ing directly to `main`
- all code changes require a pull request
- all PRs require at least 1 approval
- no bypassing of the above rules