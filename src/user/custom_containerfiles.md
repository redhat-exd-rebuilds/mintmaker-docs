# Custom Container Files

If MintMaker is not updating important container/Docker files in your project,
the reason might be the following.

Renovate's [manager for container files](https://docs.renovatebot.com/modules/manager/dockerfile/)
has a specific rule to match files:
```
(^|/|\.)([Dd]ocker|[Cc]ontainer)file$
(^|/)([Dd]ocker|[Cc]ontainer)file[^/]*$
```
note that it only matches files with specific names and at the root of the repository.

If your container/Docker file has a different name, or lies deeper in your
repository, you will need to extend the match rule.

This can be done following [these instructions](https://docs.renovatebot.com/modules/manager/#file-matching).

In a nutshell, the `fileMatch` configuration is mergeable. This means that, when
setting new values in the repository config, they will not override the default
config. Instead the new values will be merged together with the old.

For example, you could add a section like this in your `renovate.json` file:
```json
{
  "dockerfile": {
    "fileMatch": [
        "path/to/containerfile1",
        "path/to/containerfile2"
    ]
  }
}
```
