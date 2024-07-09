# Dockerfile design

MintMaker's [Dockerfile](https://github.com/konflux-ci/mintmaker-renovate-image/blob/main/Dockerfile)
is built from [ubi9-minimal](https://catalog.redhat.com/software/containers/ubi9-minimal/61832888c0d15aff4912fe0d).

The Docker image has to provide the following as a bare minimum:

- `renovate` executable
    - `node` and `npm` executables to be able to build Renovate from source
- `tkn` executable for running inside a Tekton pipeline
- `$PATH` environment variable extended with directories that contain 
  executables of different managers
- The `renovate` user under which all processes run
- `git` for cloning the source repositories

## Running the image

The working directory is `/workspace`. If running in OpenShift, it must
run as the `renovate` user with UID 1001:

```yaml
apiVersion: tekton.dev/v1beta1
kind: Task
spec:
  stepTemplate:
    workingDir: /workspace
    securityContext:
      runAsUser: 1001
```

The command to run is `renovate`. All other commands by default run
under `/bin/sh`.

## RPM lockfile support

This feature requires `skopeo`, Python, `pip` and `python3-dnf` package
present in the image.

## Python based managers

Managers such as `poetry`, `pdm` and similar require Python and `pip`,
through which [pipx](https://github.com/pypa/pipx) is installed. `pipx` is used to isolate virtual
environments so it's easier to install all required managers independent
from each other's dependencies.

Some Python based projects can require a specific Python version,
which is why the Dockerfile adds multiple Python versions via `microdnf install`.
