# Introduction

MintMaker is a service built on top of [Renovate](https://www.mend.io/renovate/). It includes
the RPM lockfile extension and integration with [Konflux CI](https://konflux-ci.dev/).

Renovate is a tool that automates dependency updates in your repository. Its goal
is to make dependency management easier and a continued practice. That way, your
project will aways have the latest fixes for bugs and security issues.

You can find interesting use cases and motivations for using Renovate in your
project [here](https://docs.renovatebot.com/user-stories/swissquote/).

MintMaker is the integration of Renovate to Konflux CI, plus an extension to
RPM lockfiles.

In this documentation, when we mention something about Renovate, it will be a
general statement. Therefore it will also apply to MintMaker, since it runs an
underlying Renovate service. On the other hand, when we say something about
MintMaker, it is a specific statement that might not apply to other Renovate 
instances.
 