---
title: Timeline
layout: post
date: 2016-06-02 17:27:47 +0200
comments: true
tags: [Risu, contribute, whatsnew]
description:
menu: main
---

**Table of contents**

<!-- TOC depthFrom:1 insertAnchor:true orderedList:true -->

- [Changelog hilights](#changelog-hilights)
- [2018-03-25](#2018-03-25)
- [2018-03-18](#2018-03-18)
- [2018-03-02](#2018-03-02)
- [2018-02-17](#2018-02-17)
- [2018-02-11](#2018-02-11)
- [2018-01-27](#2018-01-27)
- [2018-01-24](#2018-01-24)
- [2018-01-22](#2018-01-22)
- [2018-01-16](#2018-01-16)
  - [Risu](#risu)
  - [Magui](#magui)

<!-- /TOC -->

<a id="markdown-changelog-hilights" name="changelog-hilights"></a>

## Changelog hilights

This file will contain a manually mantained log of hilights between versions, it's not a very extensive detail, but some of the bigger changes/ideas will be added here.

Check [What's new]({tag}whatsnew): tag for more updated information.

<a id="markdown-2018-03-25" name="2018-03-25"></a>

## 2018-03-25

- Implement `--find` to Risu so that it can grep across a folder set for checking historic data for tests.

<a id="markdown-2018-03-18" name="2018-03-18"></a>

## 2018-03-18

- Magui autogrouping support, generating files for each comparison set like osp roles, same hostname, etc. It's based on metadata plugins generated.
  - Skip reexecution if a specified sosreport set was already analyzed.

<a id="markdown-2018-03-02" name="2018-03-02"></a>

## 2018-03-02

- Implemented 'faraday-exec' plugin to generate fake plugins that run and output metadata that later is faked via a datahook to be compared via magui plugins.
- Implemented automatic pypi.org package generation for each master merge that allows to run Risu installed via 'pip' or 'pipsi'.

<a id="markdown-2018-02-17" name="2018-02-17"></a>

## 2018-02-17

- Implemented 'profiles' data hook
  - Allow to define a text file with include/exclude filter and description that grabs data from the obtained results and shows in one place all return codes and error messages received
  - This will allow to define 'healthchecks' based on other plugins output and generate them dinamycall.
  - As it is done as if another plugin was executed, same Web UI interface is available for checking results.

<a id="markdown-2018-02-11" name="2018-02-11"></a>

## 2018-02-11

Several changes introduced recently:

- New plugins :)
- When running rerun, improved some of the logic to also copy over www so that it matches the version of the json file.
- Improved rerun, to only get results for missing plugins (unless forcerun used)
- Faraday can now accept bundle of files CSV in the list of files, it will mangle the extension reported name and description to match the file iterated.
  - This allows one file to act over several FS files (for example, `policy.json` for several services).
- UT's
  - Some other UT tweaks to ensure plugins report no data to stdout, and ability to drop bunch of jsons to run that UT over them.
  - We moved data to be a dictionary (instead of array of dictionaries), to better and faster filter on included plugins and others that are dependant on data generated (like Magui ones).
  - UT to check for tests that were doing 'echo $RC_' instead of 'exit $RC\_'
- Risu www
  - Now uses the generated 'name' for plugins so we can tune it from the framework side.
  - Also, auto switches to `magui.json` when no `Risu.json` exists, or shows a dropdown to select which one to show.

<a id="markdown-2018-01-27" name="2018-01-27"></a>

## 2018-01-27

- DevConf.cz 2018 [Detect pitfalls of osp deployments with Risu](https://devconfcz2018.sched.com/event/DJXG/detect-pitfalls-of-osp-deployments-with-Risu)
  - Recording at <https://www.youtube.com/watch?v=SDzzqrUdn5A>

<a id="markdown-2018-01-24" name="2018-01-24"></a>

## 2018-01-24

- Faraday extension

  - Some files must be equal or different across sosreports, actually we do have `release` and `ceilometer-yaml` one that rely on this, but this is hard to mantain as each new file will require a new plugin for Risu plus a new plugin for Magui.

  - In order to simplify this a new extension has been created so adding a new file to monitor no longer requires new plugins for `Risu` or `magui` but just creating a text file with some data within as documented on `Risuclient/plugins/faraday/README.md`

<a id="markdown-2018-01-22" name="2018-01-22"></a>

## 2018-01-22

- Changed the way we work with sosreports for Risu and Magui:
  - Now all plugins are always executed and filters do act on the output only.
  - If the folder is writable, Risu will write `Risu.json` to sosreport folder.
  - If there's an existing `Risu.json` it will be loaded from disk and run skipped unless there are new plugins that require execution. Forcing execution can be indicated with parameter `-r` to both Magui and Risu.

<a id="markdown-2018-01-16" name="2018-01-16"></a>

## 2018-01-16

<a id="markdown-risu" name="risu"></a>

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
  - We did extend Risu to take `--web` to automatically create the json named `Risu.json` on the folder specified with `-o` and copy the `risu.html` file there. So if you provide sosreports over http, you can point to risu.html to see graphical status! (check latest image at Risu website as [www.png](https://github.com/Risuorg/Risu/raw/master/doc/images/www.png) )
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
