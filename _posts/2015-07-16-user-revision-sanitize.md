---
layout: post
title: "User revision + entity_load + drush sqlsan = Uh Oh!"
date: 2015-07-16
author: Chris Russo
tags: Drupal
summary: The user revision module does not (_yet_) care about `drush sqlsan`, and it should!
---

One of the most embarrassing, and potentially costly, things we can do as developers
is to send emails out to real people unintentionally from a development 
environment. It happens, and often times, we aren't even aware of the fact, until the damage
is done, and a background process sends out, say 11,000 automated emails to
existing customers (actually happened to a former employer recently).

Given that reality, we need to take care of how not to do that. Often our very
first step of measure is to have a policy to always sanitize our databases, 
and in the Drupal world, that typically means using the `drush sql-sanitize` 
 command. By default, this will update your database to set all mails to a
 non-sendable address. This is a good thing.
 
    
  Other mail options: 
  maillog
  mailcatcther
  mail reroute
  devel mail - writes to local files
 
 

 
 Even in cases in which you, say, are accidentally still sending out emails 
  in an automated way, often from [cron](somewhere.com), sending to phony addresses
  is not an egregious error, since no end-user receives an email that confuses
  him/her or makes her lose confidence in your organization.
  
  So, in _most_ cases, `drush sqlsan` (alias) is enough. I did say _most_, though.
  Sadly, I'm not yet aware of a panacea of the send-emails-from-dev-environment
  sickness. Please [comment]('#js-expander-content') if you know of one!
  
  One pernicious case, in which `drush sqlsan` is insufficient, is when you have the [user_revision](https://www.drupal.org/project/user_revision )
  module enabled, at least until they incorporate [my patch]('findpatch'). The 
  user_revision module extends and loads extra files when you run
  entity_load(). 
  http://cgit.drupalcode.org/user_revision/tree/user_revision.module?id=cce42174aec453e6652da8738e397df20b6f2cd0#n164
  
  The way entity_load works, http://cgit.drupalcode.org/drupal/tree/includes/entity.inc?h=7.x#n308
  is it overwrites fields on the base table, in this case `users` with fields
  of the same name from the "revision" table, in this case `user_revisions`
  
  We lost valuable information when users canceled 
  https://github.com/chrisarusso/Tilthy-Rich-Compost-Website/issues/29
  
  So we added user revision: https://github.com/chrisarusso/Tilthy-Rich-Compost-Website/commit/fccc3f7387616510d512d3700639c5de3a560a1e
  
  Which I of course found doing git log --follow 
  
  So now when running drush sqlsan, point to commit on TRC with patched, after we run drush sqlsan
  it still has $user->mail evaluates to real mail.
  
  So, we need to hook into the drush sqlsan command, which fortunately allows that
  via : link to drush sqlsan command
  
  So, I never did it before, but switched debugger to xdebug from zendbugger, and
  set up debuggin from drush
  
  Ensured composer is up-to-date: http://whaaat.com/installing-drush-8-using-composer
https://twitter.com/brantwynn


https://www.deeson.co.uk/labs/debugging-drupal-drush-real-time-phpstorm-and-xdebug - worked to debug drush
 - setup Xdebug viw homebrew
needed to change port to 10000

http://antistatique.net/en/we/blog/2013/09/17/debugging-with-xdebug-and-phpstorm-on-macos-x 
use to debug, kosta already does this: 

https://github.com/kostajh/dotfiles/blob/master/.bashrc#L85
  
   We had been doing this for over 2 years with TRC, and found out
   - point out commit for adding user_revision
   - point to settings.php don't mail
   

Notes:

http://randyfay.com/content/remote-command-line-debugging-phpstorm-phpdrupal-including-drush
hmmmm, does .info need to be updated with drush.inc?

Will user_revision be in D8?

TODO:
- File issue on project in d.o against user_revision (or maybe drush)? [done]
- Commit to TRC project
- Write article about it [doing]
- Comment on Lullabot's recommendation [pending]
- Schedule tweet calling out lullabot [pending]





