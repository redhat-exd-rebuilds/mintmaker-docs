# Advanced Topics


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

## How to stop PRs/MRs from being updated outside of schedule

If you set up a schedule for your repo via the [`schedule`](https://docs.renovatebot.com/configuration-options/#schedule) config, and still see MintMaker updating MRs/PRs outside of the allowed times, you might need to try a different config.

The `schedule` config manages branch creation, and it will not restrain updates to MRs/PRs from branches that are already created. This is because that is a flow of an already-existing branch. 

There is another configuration called [`updateNotScheduled`](https://docs.renovatebot.com/configuration-options/#updatenotscheduled), which when set to `false` will disallow for updates in existing MRs/PRs outside of the schedule:
```json
        "updateNotScheduled": false
```

The default value of `updateNotScheduled` is `true`, which leads to this behavior that might seem unexpected at first.

## Automerge

It is possible to configure Renovate to merge updates automatically for specific
dependencies. You can find the documentation on this topic [here](https://docs.renovatebot.com/key-concepts/automerge/).

When set in a given PR/MR, an automerge will happen provided two conditions are met:
- the repositorie's CI pipeline ran successfully succeeds, and
- the PR/MR branch is up-to-date with the base branch.

Because of the need for the CI pipeline to succeed, you should expect that the
merge will only happen in the next time mintmaker is run.

This following timeline exemplifies the events leading to an automerge in a repository:

  Time  | Event
--- | --- 
__[10:00am]__ | MintMaker run 1 starts 
__[10:01am]__ | PR for dependency `xyz` in is filed
__[10:02am]__ | CI pipeline is started
__[10:05am]__ | CI pipeline finishes successfully
__[10:10am]__ | MintMaker run 1 is finished
... | ...
__[11:00am]__ | MintMaker run 2 starts
__[11:01am]__ | PR for dependency `xyz` is detected
__[11:02am]__ | PR for dependency `xyz` is automerged


Because of the need for the PR/MR branch being up-to-date with the base branch,
automerging multiple branches at once does not work. 

> **NOTE:** automerging can be risky. Since the merges will happen without anyone
> looking at the code, they have a higher risk of introducing regression.
>
> It is _very important_ to have a good test coverage in place, to mitigate that 
> risk.

You can set automerge only for a certain type of updates. For example, updates
to patch and minor updates of certain packages.

For example, to enable automerge for non-major updates on all dependencies, you
can add the following to `renovate.json`:
```json
{
  "packageRules": [
    {
      "description": "Automerge non-major updates",
      "matchUpdateTypes": ["minor", "patch"],
      "automerge": true
    }
  ]
}
```
alternatively, to enable non-major updates only for specific packages, you can use
```json
{
  "packageRules": [
    {
      "description": "Automerge non-major updates on depA and depB",
      "matchUpdateTypes": ["minor", "patch"],
      "matchPackageNames": ["depA", "depB"],
      "automerge": true
    }
  ]
}
```
See Renovate's [docs](https://docs.renovatebot.com/key-concepts/automerge/#configuration-examples)
on this topic on further examples. They show how to set automerge for specific
dependency groups, types, etc.

Finally, to check if automerge is cofigured for a given PR/MR, you can look for 
the annotation "Automerge: Enabled" in the PR/MR body.