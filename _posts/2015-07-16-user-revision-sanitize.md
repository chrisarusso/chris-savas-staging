---
layout: post
title: "User revision + entity_load + drush sqlsan = Uh Oh!"
date: 2015-07-16
author: Chris Russo
tags: Drupal
summary: The user revision module does not (_yet_) care about `drush sqlsan`, and it should!
---

One of the most embarrassing, and potentially costly things we can do as developers
is to send emails out to real people unintentionally from a development 
environment. It happens, and often times, we aren't even aware of it, until the damage
is done, and a background process sends out, say, 11,000 automated emails to
existing customers (actually happened to a former employer recently).


 In the Drupal world, there are myriad ways to attempt to address this problem. 
    
### Options
- [maillog](https://www.drupal.org/projectn /maillog) - logs mails, allows you to "not send" them.
- [mailcatcher](http://mailcatcher.me/) - Need to ask the boys how to set this one up
- [reroute email](https://www.drupal.org/project/reroute_email) - intercepts email and routes 
- [devel mail](https://api.drupal.org/api/devel/devel.mail.inc/7) - writes to local files

Given that possibility, we need to take care of how to best avoid this albeit nearly
inevitable scenario. Often, before considering one of the above solutions, the 
first step of measure is to have a policy to always sanitize our databases for development, 
and in the Drupal world, that typically means using the `drush sql-sanitize` 
 [command](http://drushcommands.com/drush-6x/sql/sql-sanitize). With no arguments,
  how I typically execute it, this will update your database to set all users emails
  addresses to a phony, non-receeivable address that looks like this: `user+1@localhost.localdomain`. 
  This is a good thing. Then, even in cases in which you do accidentally send out emails 
  in an automated way, often from [cron](https://www.drupal.org/cron), sending to phony addresses
  is a livable mistake, no end-user receives an email that confuses
  him/her or makes her lose confidence in your organization.
  
  So, in _most_ cases, `drush sqlsan` (alias) is enough, and the module or system
   options
  are not necessary. Of course, _most_ does **NOT** mean all.
  Sadly, I'm not yet aware of a panacea for the send-emails-from-dev-environment
  sickness. Please [comment](#comment-form) if you know of one!
  
  One pernicious case, in which `drush sqlsan` is insufficient, is when you have 
  the [user_revision](https://www.drupal.org/project/user_revision )
  module enabled, at least until they incorporate [my patch](https://www.drupal.org/node/2534638). The 
  user_revision module 
  [extends the `UserController`](http://cgit.drupalcode.org/user_revision/tree/user_revision.module?id=cce42174aec453e6652da8738e397df20b6f2cd0#n164) 
  class, which, due to 
    [the way that `entity_load()` works](http://cgit.drupalcode.org/drupal/tree/includes/entity.inc?h=7.x#n306) 
    the "revision" table, `user_revision` in the case, overwrites fields from the "base" table
    `users`. Therefore the user object will have the `mail` field from the `user_revision` table.

 I discovered this when adding new cron, notification functionality for 
 [Tilthy Rich Compost](http://tilthyrichcompost.com). 
 We [began using](https://github.com/chrisarusso/Tilthy-Rich-Compost-Website/commit/fccc3f7387616510d512d3700639c5de3a560a1e) the `user_revision` 
 module in 2013 due to [losing valuable information](https://github.com/chrisarusso/Tilthy-Rich-Compost-Website/issues/29
) from canceled users.
  
  So I wrote a patch,

  Which I of course found doing `git log --follow` 
  
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





