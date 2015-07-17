---
layout: post
title: "User revision + entity_load + drush sqlsan = Uh Oh!"
date: 2015-07-16
author: Chris Russo
tags: Drupal
summary: The user revision module does not (_yet_) care about `drush sqlsan`, and it should!
---

#### The general problem
One of the most embarrassing, and potentially costly things we can do as developers
is to send emails out to real people unintentionally from a development 
environment. It happens, and often times, we aren't even aware of it, until the damage
is done, and a background process sends out, say, 11,000 automated emails to
existing customers (actually happened to a former employer recently). 


 In the Drupal world, there are [myriad ways](https://github.com/chrisarusso/Tilthy-Rich-Compost-Website/commit/64a558e2)
  to [attempt to address](https://github.com/chrisarusso/Tilthy-Rich-Compost-Website/blob/master/scripts/sanitize.php) this problem. 
    
#### General solutions to the general problem
- [maillog](https://www.drupal.org/project/maillog) - A Drupal module that 
logs mails to the database and optionally allows you to "not send" them
- [mailcatcher](http://mailcatcher.me/) - Configure your local mail server to not
 send mail through PHP
- [reroute email](https://www.drupal.org/project/reroute_email) - A Drupal
module that intercepts email and routes it to a configurable address 
- [devel mail](https://api.drupal.org/api/devel/devel.mail.inc/7) - An 
option of the beloved devel module which writes emails to local files instead 
of sending

#### _The_ solution to _the problem_?
**Don't store real email addresses in your development environment**. In the Drupal world, 
that means we always sanitize our databases for development using the `drush sql-sanitize` 
 [command](http://drushcommands.com/drush-6x/sql/sql-sanitize). With no arguments,
  how I typically execute it, the command will set all users emails
  addresses to a phony, non-receivable address that looks like this: `user+1@localhost.localdomain`. 
  This is a good thing. Then, even in cases in which you do accidentally send out emails 
  in an automated way, often from [cron](https://www.drupal.org/cron), sending to phony addresses
  is a livable mistake, no end-user receives an email that confuses
  him/her or makes her lose confidence in your organization.
  
  So, in _most_ cases, `drush sqlsan` (alias) is enough, and the module or system
   options
  are not expressly necessary to avoid the worst of outcomes. Of course, _most_ does **NOT** mean all,
  which is why I'm writing.
  Sadly, I'm not yet aware of a panacea for the send-emails-from-dev-environment
  sickness. Please [comment](#comment-form) if you know of one!
  
#### **The** _specific_ problem with `user_revision` module
One pernicious case, in which `drush sqlsan` is insufficient, is when you have 
  the [user_revision](https://www.drupal.org/project/user_revision)
  module enabled, at least without [my patch](https://www.drupal.org/node/2534638) applied. The 
  user_revision module 
  [extends the `UserController`](http://cgit.drupalcode.org/user_revision/tree/user_revision.module?id=cce42174aec453e6652da8738e397df20b6f2cd0#n164) 
  class, which, due to 
    [the way that `entity_load()` works](http://cgit.drupalcode.org/drupal/tree/includes/entity.inc?h=7.x#n306) 
    the "revision" table, `user_revision` in the case, overwrites fields from the "base" table
    `users`. Therefore the user object will have the `mail` field from the `user_revision` table.


#### How did this come about?
 I discovered this when adding new cron, notification functionality for 
 [Tilthy Rich Compost](http://tilthyrichcompost.com). 
 We [began using](https://github.com/chrisarusso/Tilthy-Rich-Compost-Website/commit/fccc3f7387616510d512d3700639c5de3a560a1e) the `user_revision` 
 module in 2013 due to [losing valuable information](https://github.com/chrisarusso/Tilthy-Rich-Compost-Website/issues/29
) from canceled users. After sending emails to subscribers from my development
 environment for the 10th time in 2 years, even after sanitizing,
 I was determined to figure out once and for all, 
what was going on. So like any reasonable developer, I used my debugger to find the
aforementioned culprit, and after consulting [kosta](), we agreed, it seemed
instantiating a [drush hook]() would be the solution. However, to do this well,
I would need to debug drush execution, which, I had never done.

#### How to debug with drush (or other CLI scripts) and PHPStorm

  
#### Whatever shall we do?!
The solution was to [write a hook](https://www.drupal.org/node/2534638) 
    that ensures the `user_revision` table `mail`
    field will be effected the same as the `users` table when `drush sqlsan` is run.
    You're welcome community. 

#### Setting up a debugger for debugging php cli scripts
  Several have blogged about this before, so I'll just point theirs out
  This is the article I used
  This is the article of my dear friend, who is an excellent teacher, an
  http://randyfay.com/content/remote-command-line-debugging-phpstorm-phpdrupal-including-drush

  
  They all seemed to use xdebug, and I had previously used ZendDebugger, so I 
  first installed xdebug with homebrew via [this method](http://antistatique.net/en/we/blog/2013/09/17/debugging-with-xdebug-and-phpstorm-on-macos-x)
 This was a helpful article, because the port change to `10000` was necessary for me.
   
Then I followed 
[these instructions](https://www.deeson.co.uk/labs/debugging-drupal-drush-real-time-phpstorm-and-xdebug)
, which [Kosta adds](https://github.com/kostajh/dotfiles/blob/master/.bashrc#L85) to his `.bashrc` file to always load when debuggin  
  
  http://antistatique.net/en/we/blog/2013/09/17/debugging-with-xdebug-and-phpstorm-on-macos-x 

[kosta](team/kosta-harlan/)
  
I ensured I was using the most recent version of drush, so I [downloaded with composer](http://whaaat.com/installing-drush-8-using-composer)
(I strongly recommend perusing Brant's [about page](http://whaaat.com/about))
  
  
  So now when running drush sqlsan, point to commit on TRC with patched, after we run drush sqlsan
  it still has $user->mail evaluates to real mail.
  
  So, we need to hook into the drush sqlsan command, which fortunately allows that
  via : link to drush sqlsan command
  
  So, I never did it before, but switched debugger to xdebug from zendbugger, and
  set up debugging from drush
  
  Ensured composer is up-to-date: http://whaaat.com/installing-drush-8-using-composer
https://twitter.com/brantwynn
 
 
   

Notes:


Will user_revision be in D8?

TODO:
- File issue on project in d.o against user_revision (or maybe drush)? [done]
- Commit to TRC project
- Write article about it [doing]
- Comment on Lullabot's recommendation [pending]
- Schedule tweet calling out lullabot [pending]
- Consider making a local.settings.php template that comments out 
- Point to settings.php on TRC
-
 Which I of course found doing `git log --follow` 




