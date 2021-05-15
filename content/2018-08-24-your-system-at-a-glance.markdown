---
layout: post
title: Risu: Your system at a glance
date: 2018-08-24 23:23:58 +0200
comments: true
tags: Risu, magui, whatsnew
category: blog
description:
author: Pablo Iranzo Gómez
---

## What's new?

Other posts in this category: [What's new]({tag}whatsnew)

### System information summary

Thanks to the suggestion of a colleague, we've added 'sysinfo' as a 'profile'.

The idea of this, together with the `RC_INFO` code introduced is to leverage some scripts that report information relevant to the system being analyzed, that might or not be a problem, but still gives you `context information` about what you're trying to diagnose.

Based on this, and by adjusting `sysinfo.txt` profile, for example to create a `sysinfo-openstack.txt` or a `sysinfo-openshift.txt` and its include/exclude filters for keywords, different set of tables could be created for relevant information for diagnosis/interpretation of failed/skipped/okay tests.

Have a look on how it might look like based on current [Gerrit reviews](https://review.gerrithub.io/c/Risuorg/Risu/+/423423):

![sysinfo]({attach}images/sysinfo.jpg)

And for example, for OSP:

![sysinfo-osp]({attach}images/sysinfo-osp.jpg)

Enjoy and contribute the System information you'll like to see!

The Risu team.
