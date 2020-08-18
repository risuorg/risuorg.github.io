---
layout: post
title: Citellus GitHub Action
date: 2020-08-18 07:50:38 +0200
comments: true
tags: citellus, magui, github
category: blog
description:
author: Pablo Iranzo Gómez
---

## GitHub Actions

Github has recently included the ability to define 'actions' or 'workflows' to execute on the repositories.

The pros:

- It's defined in a yaml inside the repository
- Allows defining secrets via the web UI for tokens, etc
- Extensive marketplace of actions and easily extensible

## Citellus and GHA

We've moved some of the CI in the repository from Travis to GHA to take advantage of this approach so now (check `.github/` folder):

- Package version upload (when a new release is created, package is uploaded to `pypi`)
- Tagging of relevant versions (when `X.Y.Z` is released, new tags are added for `X.Y` and `X` pointing to the same files)
- Python unit testing
- `pre-commit` validation
- Broken link checker for documents
- Labeling of issues
- Closing old issues/PR's when not updated
- Generation of website for <https://citellus.org>
- Managing of dependencies for actions, packages
- etc

### The path to Citellus Github Action

With the recent changes in Citellus [What's new]({tag}whatsnew) like the extra plugin tree and configuration path, we were already on the path to get more Citellus automation....

<https://github.com/citellusorg/gh-action-citellus> has been created for that purpose.

It defines via the `action.yml` the steps to setup Citellus and via the `entrypoint.sh` install dependencies, sets up permissions and then runs Citellus against the defined folder to store the output and serve the report via GitHub pages web server.

You can check one repository using it at <https://github.com/iranzo/ipival/> and see the html output at <https://iranzo.github.io/ipival/>.

As it's always easier to learn by an example, check those files:

- `.citellus.conf` at <https://github.com/iranzo/ipival/blob/master/.citellus.conf> which defines the `web` output, `excludes` to rule out all the `citellusclient` plugins, defines `title` for the report and defines an `extraplugintree` in the subfolder of this repository.
- `.github/workflows/citellus.yml` at <https://github.com/iranzo/ipival/blob/master/.github/workflows/citellus.yml> which defines to run the action on each push to master branch, and also via daily cron execution, defining the following properties for the script (that will be used by the prior commented `entrypoint.sh`):

```yaml
- uses: citellusorg/gh-action-citellus@1.0
    env:
    GITHUB_TOKEN: ${{secrets.GITHUB_TOKEN}}
    SOSREPORT: test
    CONFIGPATH: "./"
```

Enjoy!
