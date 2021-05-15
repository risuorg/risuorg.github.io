---
layout: post
title: Recent changes in Magui and Risu
date: 2018-01-16 21:17:00 +0200
comments: true
tags: Risu, magui, whatsnew
category: blog
description:
---

**Table of contents**

<!-- TOC depthFrom:1 insertAnchor:true orderedList:true -->

1. [What's new?](#whats-new)
   1. [Risu](#Risu)
   2. [Magui](#magui)
2. [Wrap up!](#wrap-up)

<!-- /TOC -->

<a id="markdown-whats-new" name="whats-new"></a>

## What's new?

During recent weeks we've been coding and performing several changes to [Risu](https://iranzo.github.io/blog/2017/07/26/Risu-framework-for-detecting-known-issues/) and [Magui](https://iranzo.github.io/blog/2017/07/31/Magui-for-analysis-of-issues-across-several-hosts/).

Checking the latest logs or list of issues open and closed on github is probably not an easy task or the best way to get 'up-to-date' with changes, so I'll try to compile a few here.

First of all, we're going to present it at [Devconf.cz 2018](https://devconfcz2018.sched.com/event/DJXG/detect-pitfalls-of-osp-deployments-with-Risu), so come stop-by if assisting :-)

Some of the changes include...

<a id="markdown-Risu" name="Risu"></a>

### Risu

- New functions for bash scripts!
  - We've created lot of functions to check different things:
    - installed rpm
    - rpm over specific version
    - compare dates over X days
    - regexp in file
    - etc..
  - Functions do allow to do quicker plugin development.
- save/restore options so they can be loaded automatically for each execution
  - Think of enabled filters, excluded, etc
- metadata added for plugins and returned as dictionary
- plugin has a unique ID for all installations based on plugin relative path and plugin name
  - We do use that ID in magui to select the plugin data we'll be acting on
- plugin priority!
  - Plugins are assigned a number between 0 and 1000 that represents how likely it's going to affect your environment, and you can filter also on it with `--prio`
- extended via 'extensions' to provide support for other plugins
  - moved prior plugins to be `core` extension
  - ansible playbook support via `ansible-playbook` command
  - metadata plugins that just generate metadata (hostname, date for sosreport, etc)
- Web Interface!!
  - [David Valee Delisle](https://valleedelisle.com/) did a great job on preparing an html that loads Risu.json and shows it graphically.
  - Thanks to his work, we did extended some other features like priority, categories, etc that are calculated via Risu and consumed via Risu-www.
  - Interface can also load `magui.json` (with `?json=magui.json`) and show it's output.
  - We did extend Risu to take `--web` to automatically create the json named `Risu.json` on the folder specified with `-o` and copy the `Risu.html` file there. So if you provide sosreports over http, you can point to Risu.html to see graphical status! (check latest image at Risu website as [www.png](https://github.com/Risuorg/Risu/raw/master/doc/images/www.png) )
- Increased plugin count!
  - Now we do have more than 119 across different categories
  - A new plugin in python `reboot.py` that checks for unexpected reboots
  - Spectre/Meltdown security checks!

<a id="markdown-magui" name="magui"></a>

### Magui

- If there's an existing `Risu.json` magui does load it to speed it up process across multiple sosreports.
- Magui can also use `ansible-playbook` to copy Risu program to remote host and run there the command, and bring back the generated `Risu.json` so you can quickly run Risu across several hosts without having to manually perform operations or generate sosreports.
- Moved prior data to two plugins:
  - `Risu-outputs`
    - Risu plugins output arranged by plugin and sosreport
  - `Risu-metadata`
    - Outputs metadata gathered by `metadata` plugins in Risu arranged by plugin and sosreport
- First plugins that compare data received from Risu on global level
  - Plugins are written in python and use each plugin `id` to just work on the data they know how to process
  - `pipeline-yaml`
    - Checks if pipeline.yaml and warns if is different across hosts
  - `seqno`
    - Checks latest galera seqno on hosts
  - `release`
    - Reports RHEL release across hosts and warns if is different across hosts
- Enable `quiet` mode on the data received from Risu as well as local plugins, so only outputs with ERROR or different output on sosreports is shown, even on magui plugins.

<a id="markdown-wrap-up" name="wrap-up"></a>

## Wrap up!

As you can see we've been busy trying to improve plugins, Risu framework and Magui as well.

We've been also busy demonstrating to others it's value and raising lot of new issues and closing them with our commits (294 requests closed so far).

So, come and [tell us](https://github.com/Risuorg/Risu/issues/new) what else are you missing or how can we improve it to suit your needs (or code them yourself and submit a review!)
