# Configuration

The base configuration can be found [here](https://github.com/konflux-ci/mintmaker/blob/main/config/renovate/renovate.json). This config file
is applied to all repositories onboarded in Konflux.

Individual repositories can override this config to tweak
Renovate's behavior to their needs by adding a `renovate.json`
file in the top level directory of the repository.
All available configuration options can be found in 
[Renovate documentation](https://docs.renovatebot.com/configuration-options/).

## Offboarding a repository

If you intend to disable MintMaker for your repository, please follow
this guide.

### Prerequisites

- Ensure you have CLI access to the Konflux cluster where your component is created.
- Ensure you have necessary permission to annotate a component.

### Steps

- Determine the Konflux component you want to off-boarding from Mintmaker.
- Use the `kubectl` or `oc` command to add the annotation `mintmaker.appstudio.redhat.com/disabled: "true"` to the component.

Example:

```bash
oc -n <namespace> annotate component/<component-name> mintmaker.appstudio.redhat.com/disabled=true
```

> **NOTE**: namespace name is different with your workspace name, for example,
  if your workspace name is `myworkspace`, then the namespace name should be 
  `myworkspace-tenant`.
