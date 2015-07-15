---
layout: post
title: "User revision + entity_load + drush sqlsan = Uh Oh!"
date: 2015-07-16
author: Chris Russo
tags: Drupal
summary: The user revision module does not care about `drush sqlsan`, and it should!
---

Notes:
/includes/entity.inc replaces any field in revision field with field from base file
lines 308 to 314

So mail is replace, but not on drush sqlsan

Can we hook into sqlsan and check for user_revision?

entity load is killing it.

TODO:
- File issue on project in d.o against user_revision (or maybe drush)?
- Write article about it
- Comment on Lullabot's recommendation
- Schedule tweet calling out lullabot
