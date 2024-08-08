# Configuration

Renovate is a very configurable tool. Since every project has its unique
needs, it is fundamental to take advantage of that flexibility to make MintMaker
work in the best possible manner.

The MintMaker team provides a base set of configurations that provide a sensible
starting point. It enables users to have a good experience out of the box.
In case you would like to tweak MintMaker behavior and need some help, feel free
to reach out to the MintMaker team in our Slack channel `#renovate-users`.

## Create your custom configuration

The base configuration can be found [here](https://github.com/konflux-ci/mintmaker/blob/main/config/renovate/renovate.json). This config file
is applied to all repositories onboarded in Konflux.

Individual repositories can override this config to tweak
Renovate's behavior to their needs by adding a `renovate.json`
file in the top level directory of the repository.
All available configuration options can be found in 
[Renovate documentation](https://docs.renovatebot.com/configuration-options/).

When customizing your configs, it is a important to inherit from the base
configuration. This can be done via the `extends` option:
```json
"extends": [
    "github>konflux-ci/mintmaker//config/renovate/renovate.json"
]
```

