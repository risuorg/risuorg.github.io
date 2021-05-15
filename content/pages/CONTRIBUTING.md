---
title: Contributing
layout: post
date: 2016-06-02 17:27:47 +0200
comments: true
tags: Risu, contribute
description:
---

**Table of contents**

<!-- TOC depthFrom:1 insertAnchor:true orderedList:true -->

1. [How to file a bug report](#how-to-file-a-bug-report)
2. [How to contribute code](#how-to-contribute-code)
3. [How to write tests](#how-to-write-tests)
4. [How to debug your test](#how-to-debug-your-test)

<!-- /TOC -->

<a id="markdown-how-to-file-a-bug-report" name="how-to-file-a-bug-report"></a>

# How to file a bug report

If you have any suggestions for improvements please do not hesitate to
open an [issue](https://github.com/Risuorg/Risu/issues/new).

<a id="markdown-how-to-contribute-code" name="how-to-contribute-code"></a>

# How to contribute code

We encourage you to contribute new plugins. We use [gerrithub][] for
reviewing proposed changes. The submission process looking something
like this:

[gerrithub]: https://gerrithub.io/

1.  Clone the Risu repository:

        git clone https://github.com/Risuorg/Risu.git

2.  Configure the `git-review` tool:

        git-review -s

3.  Check out a branch in which to make your changes:

        git checkout -b "your-new-branch"

4.  Edit your files and validate with tox:

        tox # this will check the changes for some errors

    1. NOTE: tox will run python 2.7, pep8 and python 3.5 tests, if your environment lacks for example python 3.5, do execute tox -l to see the available tests and skip that one, for example:

       ```sh
       tox -e pep8
       tox -e py27
       # We're skipping tox -e py35 which is also invoked by default when tox is executed without arguments.
       ```

    2. In this way, errors that might be relevant to the test environment can be skipped. For example this one reported at issue tracker as [104](https://github.com/Risuorg/Risu/issues/104))

5.  Update your local repository:

        git add $modified_files
        git commit

        For the message, please use a short line with the fix and the subject like `[plugins][openstack][nova] Check nova configuration XXX`

        If the commit fixes a github open issue, also use `Closes #$ISSUEID` so github automatically closes it once merged referencing the commit.

6.  Submit your changes for review:

        git-review

Then wait for your changes to be reviewed. It is common for reviewers
to request changes; when this happens:

1.  Edit your files and revalidate with tox:

        tox # this will check the new changes for some errors

2.  Update your existing commit. Do not create a new commit!

        git add $modified_files
        git commit --amend

3.  Resubmit the change:

        git-review

You can see pending and already merged actual changes at: <https://review.gerrithub.io/#/q/project:Risuorg//Risu>

4. Once the new plugin has been submitted you'll see some comments from 'Risu Jenkins' which is running Unit tests against it (same ones that you run with `tox`)

5. If Jenkins gives 'Verified +1', next step is wait for one reviewer to give final ACK and merge the change.

<a id="markdown-how-to-write-tests" name="how-to-write-tests"></a>

# How to write tests

Please refer to the
[templates](https://github.com/Risuorg/Risu/tree/master/doc/templates)
folder for examples.

If you want to contribute also Unittests for your plugins, check [TESTING.md](TESTING.md)

For contributing translations check [i18n.md](doc/i18n.md)

<a id="markdown-how-to-debug-your-test" name="how-to-debug-your-test"></a>

# How to debug your test

We've included a file named `env-for.debug.sh` in the root folder of Risu repo that allows to be sourced and later execute your script.

This environment file will define the standard variables Risu does use like:

- RC_OKAY
- RC_FAILED
- RC_SKIPPED
- TEXTDOMAIN
- TEXTOMAINDIR
- CITELLUS_BASE
- CITELLUS_LIVE
- CITELLUS_ROOT

And will preload the common-functions

The way to use it is:

```sh
. ~/Risu/env-for-debug.sh
```

Then you can debug your script with:

```
sh -x /path/to/your/plugin.sh
```

Or test individual functions output like:

```
is_rpm qemu-kvm-rhev
```

Please, do note that as some functions do 'exit' or 'return' for example executing `is_rpm_over` will exit the active shell, so beware!!
