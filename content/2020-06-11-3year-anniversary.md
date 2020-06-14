---
layout: post
title: Citellus: 3 year anniversary
date: 2020-06-11 07:55:38 +0200
comments: true
tags: citellus, magui
category: blog
description:
author: Robin Černín
---

## Time flies!

![We're 3 year old]({attach}images/3year.jpg)

Citellus has been now around for three years!

On June 11th 2017 I have created this project for automating manual checks that I had to perform over and over again. It was very simple shell script that iterated over directories and executed shell scripts in them.

Soon after that, friends, colleagues started to use the program as well and provided feedback. We have reached out for help to one of our colleague Lars and asked if he had any ideas he could share with us to improve the program.

We have realized that current form of the program is very limiting and we wanted to improve.

Pablo has created a completely new framework in python that was based on the same idea, but he identified the problems with the current and addressed them.

There were lots of improvements, especially in execution of the scripts, we started with sequential run of the scripts and moved to parallel using python.

After a while our colleagues from U.S. have noticed the program with the new improvements, but it wasn't great user experience to read the output from the CLI, especially as the number of plugins has grown to several hundreds.

David Vallee Delisle, DVD created a dashboard that allowed to read the reports of the rules in the browser and greatly improved usability.

Pablo continued working on the framework and forever improving it and adding more features, one of the big ones was M.A.G.U.I. that stands for Multiple Analysis Generic Unifier and Interpreter that allowed running Citellus on top of cluster of multiple nodes.

For a while these two were separated programs, but we realised that M.A.G.U.I. brought benefits into Citellus and they both merged under Citellus.

We have spent probably a thousand of hours or more of our free time that we have dedicated into writing this program and some of the internal rules or public.

During all this time we have had contributors that came and went, and some are still contributing to the project. I would like to shout out to Mikel and his contribution and his time.

The most important thing with this project is that even after three years, Citellus is still important.

During this period we've enhanced Citellus to be integrated as described in the [What's new]({tag}whatsnew) documents.

Happy birthday Citellus!!
