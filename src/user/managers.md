# Supported managers

This page lists the currently supported dependency managers in MintMaker.
A single manager typically manages one type of a dependency file and its
lockfile.

## RPM lockfiles

- [rpm via rpm-lockfile-prototype](https://github.com/konflux-ci/rpm-lockfile-prototype)

## CI/CD

- [tekton](https://docs.renovatebot.com/modules/manager/tekton/)

## Docker

- [dockerfile](https://docs.renovatebot.com/modules/manager/dockerfile/)

## Git

- [git-submodules](https://docs.renovatebot.com/modules/manager/git-submodules/)

## Custom

Adds ability to create a custom manager by configuring `customManagers` section
in your `renovate.json` file.

- [regex](https://docs.renovatebot.com/modules/manager/regex/)
