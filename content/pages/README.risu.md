---
title: Risu
layout: post
date: 2016-06-02 17:27:47 +0200
comments: true
tags:
description:
---

<img src="../images/Risu-mini.png" width="20%" border=0 align="right" alt='Risu logo'>

**Table of contents**

<!-- TOC depthFrom:1 insertAnchor:true orderedList:true -->

- [Introduction](#introduction)
- [Highlights](#highlights)
- [Installation](#installation)
- [Usage help](#usage-help)
- [Plugins and their descriptions](#plugins-and-their-descriptions)
- [Doing a live check example](#doing-a-live-check-example)
- [Doing a fs snapshot check example](#doing-a-fs-snapshot-check-example)
- [HTML Interface](#html-interface)
- [Ansible playbooks](#ansible-playbooks)

<!-- /TOC -->

<a id="markdown-introduction" name="introduction"></a>

## Introduction

Risu is a program that should help with system configuration validation on either live system or any sort of snapshot of the filesystem.

Via execution of 'plugins' it reports status on each one execution against the system that gives you an idea on health status, actual problems or problems that will reveal themselves if no preventive action is taken.

Please if you have any idea on any improvements please do not hesitate to open an issue.

<a id="markdown-highlights" name="highlights"></a>

## Highlights

- Plugins written in your language of choice.
- Allows to dump output to json file to be processed by other tools.
  - Allow to visualize html from json output.
  - Check our sample at: [Risu-www](/Risu.html)
- Ansible playbook support.
- Save / restore default settings

Check latest changes on [Changelog.md]({filename}Changelog.md)

Check for plugins listing on [Risuclient/plugins/](Risuclient/plugins/)

<a id="markdown-installation" name="installation"></a>

## Installation

- Just clone the git repository and execute it from there 'or'
- use 'pipsi' or create a python virtual env to install package 'Risu'
  ```sh
  # pipsi install Risu
  Already using interpreter /usr/bin/python3
  Using base prefix '/usr'
  New python executable in /home/iranzo/.local/venvs/Risu/bin/python3
  Also creating executable in /home/iranzo/.local/venvs/Risu/bin/python
  Installing setuptools, pip, wheel...done.
  Collecting Risu
  Installing collected packages: Risu
  Successfully installed Risu-0.1.0.dev1072
    Linked script /home/iranzo/.local/bin/Risu.py
    Linked script /home/iranzo/.local/bin/magui.py
  Done.
  ```
  - Pipsi will take care of installing a virtual environment and link to binary folder so you can call Risu.py or magui.py directly
  - Remember that pypi package might not contain all the latests plugins features as the github repo one.
- Container:
  - Use our automatically built container in docker hub:
    - `docker run --user=$(id -u) --rm -v $PATHTOSOSREPORT:/data:Z Risu/Risu:latest /data`
  - or build your own using the included `Dockerfile` in the git checkout.
    - `docker build . -f Dockerfile.centos7-atomic -t Risu:latest` # (from git checkout, then note image id)
    - `docker run --user=$(id -u) --rm -v $PATHTOSOSREPORT:/data:Z Risu:latest /data`
  - Notes about using docker:
    - Docker passes as volume the path specified under /data so we do use that parameter with Risu for running the tests.
    - The default user id within the container is 10001 and the commands or sosreport permissions doesn't allow that user to gather all the information, so the container is required to run as the current user.

<a id="markdown-usage-help" name="usage-help"></a>

## Usage help

We are developing framework in python, the bash framework has been deprecated. Python framework is the only supported framework.

```
usage: Risu.py [arguments] [-h] [-l] [--list-plugins] [--list-extensions]
                               [--list-categories] [--description]
                               [--list-hooks] [--output FILENAME] [--web]
                               [--run] [--find] [--blame] [--lang]
                               [--only-failed] [-v]
                               [-d {INFO,DEBUG,WARNING,ERROR,CRITICAL}] [-q]
                               [-i SUBSTRING] [-x SUBSTRING] [-p [0-1000]]
                               [-hf SUBSTRING] [--dump-config] [--no-config]
                               [sosreport]

Risu allows to analyze a directory against common set of tests, useful for
finding common configuration errors

positional arguments:
  sosreport

optional arguments:
  -h, --help            show this help message and exit
  -l, --live            Work on a live system instead of a snapshot
  --list-plugins        Print a list of discovered plugins and exit
  --list-extensions     Print a list of discovered extensions and exit
  --list-categories     With list-plugins, also print a list and count of
                        discovered plugin categories
  --description         With list-plugins, also outputs plugin description
  --list-hooks          Print a list of discovered hooks and exit
  --output FILENAME, -o FILENAME
                        Write results to JSON file FILENAME
  --web                 Write results to JSON file Risu.json and copy html
                        interface in path defined in --output
  --run, -r             Force run of Risu instead of reading existing
                        'Risu.json'
  --find                Use provided path at starting point for finding
                        Risu.json and print them based on filters defined

Output and logging options:
  --blame               Report time spent on each plugin
  --lang                Define locale to use
  --only-failed, -F     Only show failed tests
  -v, --verbose         Increase verbosity of output (may be specified more
                        than once)
  -d {INFO,DEBUG,WARNING,ERROR,CRITICAL}, --loglevel {INFO,DEBUG,WARNING,ERROR,CRITICAL}
                        Set log level
  -q, --quiet           Enable quiet mode

Filtering options:
  -i SUBSTRING, --include SUBSTRING
                        Only include plugins that contain substring
  -x SUBSTRING, --exclude SUBSTRING
                        Exclude plugins that contain substring
  -p [0-1000], --prio [0-1000]
                        Only include plugins are equal or above specified prio
  -hf SUBSTRING, --hfilter SUBSTRING
                        Only include hooks that contain substring

Config options:
  --dump-config         Dump config to console to be saved into file
  --no-config           Do not read configuration from file /home/iranzo/DEVEL
                        /Risu/Risuclient/Risu.conf or
                        ~/.Risu.conf

```

Check how does it look in an execution at:
<a href="https://asciinema.org/a/169814"><img src="https://asciinema.org/a/169814.png" width="100%" border=0  alt='Risu demo'></a>

<a id="markdown-plugins-and-their-descriptions" name="plugins-and-their-descriptions"></a>

## Plugins and their descriptions

This is new feature of Risu that will show you available scripts and their description.

```
./Risu.py --list-plugins --description
{'backend': 'core', 'description': 'This plugin checks if Apache reaches its MaxRequestWorkers', 'plugin': '/home/iranzo/DEVEL/Risu/Risuclient/plugins/core/bugzilla/httpd/1406417.sh'}
{'backend': 'core', 'description': 'Checks missconfigured host in nova vs hostname', 'plugin': '/home/iranzo/DEVEL/Risu/Risuclient/plugins/core/bugzilla/openstack/ceilometer/1483456.sh'}
{'backend': 'core', 'description': 'Checks for outdated ceph packages', 'plugin': '/home/iranzo/DEVEL/Risu/Risuclient/plugins/core/bugzilla/openstack/ceph/1358697.sh'}
{'backend': 'core', 'description': 'Checks httpd WSGIApplication defined to avoid wrong redirection', 'plugin': '/home/iranzo/DEVEL/Risu/Risuclient/plugins/core/bugzilla/openstack/httpd/1478042.sh'}
{'backend': 'core', 'description': 'Checks for keystone transaction errors on cleanup', 'plugin': '/home/iranzo/DEVEL/Risu/Risuclient/plugins/core/bugzilla/openstack/keystone/1473713.sh'}
{'backend': 'core', 'description': 'Checks for keystone LDAP domain template problem', 'plugin': '/home/iranzo/DEVEL/Risu/Risuclient/plugins/core/bugzilla/openstack/keystone/templates/1519057.sh'}
{'backend': 'core', 'description': 'Checks for wrong auth_url configuration in metadata_agent.ini', 'plugin': '/home/iranzo/DEVEL/Risu/Risuclient/plugins/core/bugzilla/openstack/neutron/1340001.sh'}
{'backend': 'core', 'description': 'Checks python-ryu tracebacks', 'plugin': '/home/iranzo/DEVEL/Risu/Risuclient/plugins/core/bugzilla/openstack/neutron/1450223.sh'}
```

<a id="markdown-doing-a-live-check-example" name="doing-a-live-check-example"></a>

## Doing a live check example

This is an example of execution of Risu using all openstack and pacemaker tests collections.

```
./Risu.py -q -l -i pacemaker -i openstack
INFO:Risu:using default plugin path
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/ceilometer_bug_1483456.sh: failed
    https://bugzilla.redhat.com/show_bug.cgi?id=1483456
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/ceph_bug_1358697.sh: failed
    outdated ceph packages: https://bugzilla.redhat.com/show_bug.cgi?id=1358697
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/httpd_bug_1478042.sh: skipped
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/keystone_bug_1473713.sh: okay
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/neutron_bug_1450223.sh: skipped
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/neutron_bug_1474092.sh: okay
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/neutron_bug_1489066.sh: okay
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/nova_bug_1474092.sh: okay
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/swift_bug_1500607.sh: failed
    swift expirer https://bugzilla.redhat.com/show_bug.cgi?id=1500607
# /root/Risu/Risuclient/plugins/core/launchpad/openstack/keystone_bug_1649616.sh: okay
# /root/Risu/Risuclient/plugins/core/openstack/ceilometer/expiration.sh: failed
    ceilometer.conf setting must be updated:
    alarm_history_time_to_live = -1
    ceilometer.conf setting must be updated:
    event_time_to_live = -1
    ceilometer.conf setting must be updated:
    metering_time_to_live = -1
```

<a id="markdown-doing-a-fs-snapshot-check-example" name="doing-a-fs-snapshot-check-example"></a>

## Doing a fs snapshot check example

This is an example of execution of Risu using `pacemaker` and `openstack` filter against fs snapshot.

```
./Risu.py -q -i pacemaker -i openstack sosreport-undercloud-0.redhat.local-20171117212710/
INFO:Risu:using default plugin path
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/ceilometer_bug_1483456.sh: failed
    https://bugzilla.redhat.com/show_bug.cgi?id=1483456
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/ceph_bug_1358697.sh: failed
    outdated ceph packages: https://bugzilla.redhat.com/show_bug.cgi?id=1358697
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/httpd_bug_1478042.sh: skipped
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/keystone_bug_1473713.sh: okay
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/neutron_bug_1450223.sh: skipped
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/neutron_bug_1474092.sh: okay
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/neutron_bug_1489066.sh: okay
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/nova_bug_1474092.sh: okay
# /root/Risu/Risuclient/plugins/core/bugzilla/openstack/swift_bug_1500607.sh: failed
    swift expirer https://bugzilla.redhat.com/show_bug.cgi?id=1500607
# /root/Risu/Risuclient/plugins/core/launchpad/openstack/keystone_bug_1649616.sh: okay
# /root/Risu/Risuclient/plugins/core/openstack/ceilometer/expiration.sh: failed
    ceilometer.conf setting must be updated:
    alarm_history_time_to_live = -1
    ceilometer.conf setting must be updated:
    event_time_to_live = -1
    ceilometer.conf setting must be updated:
    metering_time_to_live = -1
```

<a id="markdown-html-interface" name="html-interface"></a>

## HTML Interface

- Create by using `--output $FOLDER` and `--web`, open the generated `Risu.html`.

<img src="../images/www.png" height="40%" border=0 alt='WWW interface capture'>

- Risu-web now supports the parsing of magui.json.

- It's possible to tell the Risu.html which json to parse by adding json=<jsonfile> as a query string:

```
http://host/Risu.html?json=magui.json
```

<a id="markdown-ansible-playbooks" name="ansible-playbooks"></a>

## Ansible playbooks

Risu can also run Ansible playbooks via extension

The are some additional conventions that are detailed in [ansible-playbooks.md](doc/ansible-playbooks.md) that determine how to code them to be executed in live or snapshoot mode.

Commands have been extended to allow `--list-plugins` to list them and include /exclude filters to work with them.

All of them must end in `.yml`.

```
found #1 extensions / found #0 tests at default path
mode: fs snapshot .
# Running extension ansible-playbook
# /home/iranzo/DEVEL/Risu/Risu/playbooks/system/clock-ntpstat.yml: skipped
    Skipped for incompatible operating mode
```

vs

```
found #2 extensions with #2 plugins
mode: live
# /home/iranzo/DEVEL/Risu/Risuclient/plugins/ansible/openstack/rabbitmq/ha-policies.yml: okay
# /home/iranzo/DEVEL/Risu/Risuclient/plugins/ansible/system/clock-ntpstat.yml: failed
    {"changed": false, "cmd": "ntpstat", "msg": "[Errno 2] No such file or directory",

```
