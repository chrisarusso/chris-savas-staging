---
layout: post
title: "git'n ish done!"
date: 2015-07-01
author: Chris Russo
tags: git efficiency
summary: A few quick git examples and use-cases , round #1
---

Holy git you can do a lot with this tool.

You're familiar, you use it, but want to use it better.

I just updated via [homebrew](http://brew.sh) to the latest official release of 
git: 2.4.5

{% highlight bash %}

MacBook-Pro:savas-site chris$ brew upgrade git
==> Upgrading 1 outdated package, with result:
git 2.4.5
==> Upgrading git
==> Downloading https://homebrew.bintray.com/bottles/git-2.4.5.yosemite.bottle.tar.gz
######################################################################## 100.0%
==> Pouring git-2.4.5.yosemite.bottle.tar.gz
{% endhighlight %}


Which not provides me with the following git commands, and we'll only scratch
the surface of using primarily the most common ones 

{% highlight bash %}

MacBook-Pro:savas-site chris$ git help -a
usage: git [--version] [--help] [-C <path>] [-c name=value]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

available git commands in '/usr/local/Cellar/git/2.4.5/libexec/git-core'

  add                       commit-tree               fsck                      merge-index               receive-pack              show-branch
  add--interactive          config                    fsck-objects              merge-octopus             reflog                    show-index
  am                        count-objects             gc                        merge-one-file            relink                    show-ref
  annotate                  credential                get-tar-commit-id         merge-ours                remote                    stage
  apply                     credential-cache          grep                      merge-recursive           remote-ext                stash
  archimport                credential-cache--daemon  gui                       merge-resolve             remote-fd                 status
  archive                   credential-store          gui--askpass              merge-subtree             remote-ftp                stripspace
  bisect                    cvsexportcommit           hash-object               merge-tree                remote-ftps               submodule
  bisect--helper            cvsimport                 help                      mergetool                 remote-http               svn
  blame                     cvsserver                 http-backend              mktag                     remote-https              symbolic-ref
  branch                    daemon                    http-fetch                mktree                    remote-testsvn            tag
  bundle                    describe                  http-push                 mv                        repack                    unpack-file
  cat-file                  diff                      index-pack                name-rev                  replace                   unpack-objects
  check-attr                diff-files                init                      notes                     request-pull              update-index
  check-ignore              diff-index                init-db                   p4                        rerere                    update-ref
  check-mailmap             diff-tree                 instaweb                  pack-objects              reset                     update-server-info
  check-ref-format          difftool                  interpret-trailers        pack-redundant            rev-list                  upload-archive
  checkout                  difftool--helper          log                       pack-refs                 rev-parse                 upload-pack
  checkout-index            fast-export               ls-files                  patch-id                  revert                    var
  cherry                    fast-import               ls-remote                 prune                     rm                        verify-commit
  cherry-pick               fetch                     ls-tree                   prune-packed              send-email                verify-pack
  citool                    fetch-pack                mailinfo                  pull                      send-pack                 verify-tag
  clean                     filter-branch             mailsplit                 push                      sh-i18n--envsubst         web--browse
  clone                     fmt-merge-msg             merge                     quiltimport               shell                     whatchanged
  column                    for-each-ref              merge-base                read-tree                 shortlog                  write-tree
  commit                    format-patch              merge-file                rebase                    show


{% endhighlight %}


# Reading

#### git branch -av
**Description**: The git branch command shows what branches have been created.
The -a option shows remote and local branches and the -v option shows the 
corresponding commit hash.  
**Use case**: A quick overview of where local and remote branches are 
[in sync](http://www.nsync.com/) or differ.  
**Example**:  In writing this blog post, you see me on a new branch 
'advanced-git', which doesn't yet exist on our team repository `remotes/github`
and you can also see that there are several branches I don't have created
locally (`drupal-planet`), which exist on a remote repository.

{% highlight bash %}
MacBook-Pro:savas-site chris$   git branch -av
  58-add-photos                                    861543b Feature #58: Add a photo to the home page with basic responsiveness
  add-blockquote-border                            7fc3a6a Add simple blockquote styling. To be updated more later.
* advanced-git                                     3d27ae2 Switch comments on
  ctools-block                                     82c5312 Add link to example module repo, clarify the example-form path
  master                                           3d27ae2 Switch comments on
  update-posts                                     20119b6 Add links to CSS post and fix PHP highlighting on D8 theming post
  remotes/github/58-add-photos                     861543b Feature #58: Add a photo to the home page with basic responsiveness
  remotes/github/SAV-46-homepage-blogs             10e9072 SAV-46: only show 3 most recent posts
  remotes/github/drupal-planet                     3e7524f Make relative links absolute in drupal posts
  remotes/github/mapping-in-d8-blab                19a3d34 Refine mapping blab and add lots of images
  remotes/github/master                            3d27ae2 Switch comments on
  remotes/github/update-posts                      20119b6 Add links to CSS post and fix PHP highlighting on D8 theming post
  remotes/kostajh/ctools-block                     82c5312 Add link to example module repo, clarify the example-form path
  remotes/kostajh/master                           4efd1be Go back to old drupal.org/u/{username} syntax
{% endhighlight %}

#### git blame
**Description**: The git blame command shows who did what, when, why and how, line
by-line. It's the ultimate blame-game and I **_LOVE_** it.    
**Use case**: Someone wrote custom code doing something complex but did not 
document it at all, and now you have to extend it.
**Example**:  Let's take a look at our [team](/team) page from a code 
perspective. We can see that though there are no comments in this case,
 I am the one who added the description of our company name Savas, and that transcended a few
 commits 
 ([6a9f3362](https://github.com/savaslabs/savaslabs.github.io/commit/6a9f3362), 
 [b7df5afc](https://github.com/savaslabs/savaslabs.github.io/commit/b7df5afc),
  [7e3d037f](https://github.com/savaslabs/savaslabs.github.io/commit/7e3d037f)) 
  as I am wont to do. 

{% highlight bash %}
fa23bf0a (Chris Russo     2015-06-17 15:04:55 -0400 32) <div id="what-does-savas-mean" class="name-explanation wrapper column-8">
fce6d2ae (Chris Russo     2015-05-11 15:43:47 -0400 33)     <h3>What does Savas mean?</h3>
6a9f3362 (Chris Russo     2015-05-05 16:26:32 -0400 34)     <p>
b7df5afc (Chris Russo     2015-05-07 18:19:53 -0400 35)         The name Savas derives its root from the French word <i>savoir</i> which means "to know". Savas is also a <a href="http://en.wikipedia.org/wiki/Palindrome" target="_blank" >palindrome</a>, 
6a9f3362 (Chris Russo     2015-05-05 16:26:32 -0400 36) which means it is read forward and backward the same, and implies balance and symmetry. Together, an informed, and balanced approach speaks to how we collaborate with our clients to produce sound solutions.
7e3d037f (Chris Russo     2015-06-22 17:20:52 -0400 37)         To learn more read <a href="/2015/06/23/a-man-a-plan-a-canal-a-savas.html">this post</a>.
6a9f3362 (Chris Russo     2015-05-05 16:26:32 -0400 38)     </p>
6a9f3362 (Chris Russo     2015-05-05 16:26:32 -0400 39) </div>
7d62ad28 (Anne Tomasevich 2015-04-07 17:34:36 -0400 40) <div class="associations wrapper">
7d62ad28 (Anne Tomasevich 2015-04-07 17:34:36 -0400 41)     <h3>Associations and Certifications</h3>
7d62ad28 (Anne Tomasevich 2015-04-07 17:34:36 -0400 42)     <div class="logos">
bfd73e12 (Kosta Harlan    2015-04-14 14:41:15 -0400 43)         <a href="https://www.drupal.org/node/2466865"><img src="/assets/img/drupal_assoc_member_logo.png" width="120px" height="120px" alt="Drupal Association logo"></a>
70f0ef55 (Kosta Harlan    2015-04-08 10:42:05 -0400 44)         <a href="http://www.durhamlivingwage.org/"><img src="/assets/img/durham_living_wage_logo.png" width="136px" height="85px" alt="Durham Living Wage logo"></a>
7d62ad28 (Anne Tomasevich 2015-04-07 17:34:36 -0400 45)     </div>
70f0ef55 (Kosta Harlan    2015-04-08 10:42:05 -0400 46) </div>
{% endhighlight %}

### git branch --contains
**Description**: The git branch --contains option shows the branches that 
contain a given commit.    
**Use case**: Show me all current branches that are branched off of the most 
 recent commit on master.
**Example**:  Let's take a look to see if anyone else is up to date with the 
latest and greatest on their branches, or if they need to rebase

{% highlight bash %}
MacBook-Pro:savas-site chris$ git branch
  58-add-photos
  add-blockquote-border
* advanced-git
  ctools-block
  master
  update-posts
MacBook-Pro:savas-site chris$ git branch --contains github/master 
* advanced-git
  master
{% endhighlight %}

  
### git log --follow
**Description**: The git --follow option shows the history of a given file it 
 tracks since the file may move from tree to tree within the repository.   
**Use case**: If you are wise, you will move a Drupal module that you've patched
 into a `patched` directory under `sites/all/modules` to avoid auto-overwriting
 it when you do module upgrades. Once you've done that, doing `git log` in the 
 patched directory will only show you history of those files since they were
 moved into the patched directory, but there may be useful information that
 precedes that point in time. 
**Example**: The touring-cyclist hospitality network 
[Warmshowers](https://www.warmshowers.org/) is in the middle of a Drupal 6 to
 Drupal 7 upgrade we're assisting with. We have made patches to modules over
 the years for various reasons. Our methodologies improved over time, and it 
 looks like we may need no patches for the Drupal 7 version. However, here's an 
 example of what `git log` vs `git log --follow` will show when looking at
 the patched cck module in Drupal 6.
 
 {% highlight bash %}

 MacBook-Pro:patched chris$ git log cck/
 commit 809f63b3f0e67583013727625338148c9a66d4d9
 Author: Randy Fay <randy@randyfay.com>
 Date:   Wed Jun 17 23:06:05 2015 -0600
 
     Update CCK module for security release to 6.x-2.10 for #577
     
     Update information last refreshed: Wed, 2015-06-17 22:57
      Name           Installed Version  Proposed version  Message
      Content (cck)  6.x-2.9            6.x-2.10          SECURITY UPDATE available
 
 commit 664528e06179d04eafefdcb9de78d663d90ea5c9
 Author: Kosta Harlan <kostajh@gmail.com>
 Date:   Sat Nov 16 13:21:08 2013 -0500
 
     Issue #348 - Move Drupal core into docroot/
{% endhighlight %}

vs.
{% highlight bash %}
MacBook-Pro:patched chris$ git log --follow cck/content.info
commit 809f63b3f0e67583013727625338148c9a66d4d9
Author: Randy Fay <randy@randyfay.com>
Date:   Wed Jun 17 23:06:05 2015 -0600

    Update CCK module for security release to 6.x-2.10 for #577
    
    Update information last refreshed: Wed, 2015-06-17 22:57
     Name           Installed Version  Proposed version  Message
     Content (cck)  6.x-2.9            6.x-2.10          SECURITY UPDATE available

commit 664528e06179d04eafefdcb9de78d663d90ea5c9
Author: Kosta Harlan <kostajh@gmail.com>
Date:   Sat Nov 16 13:21:08 2013 -0500

    Issue #348 - Move Drupal core into docroot/

commit 17c9dd2f5ff5dce1bfbdc5a80702bb5ddc2a650d
Author: koleary <kevinpatrickoleary@gmail.com>
Date:   Mon Oct 24 11:05:01 2011 -0700

    enabled HTML mail + updated CCK and Date_api modules

commit fab827305687821b87f69bb3b5cd7228e71e6088
Author: Randy Fay <rfay@hardy1.thefays.us>
Date:   Thu Aug 12 07:34:12 2010 -0600

    updates

commit 49041b670fafc3849ee93596dd38d9dd215e8483
Author: rfay <randy@randyfay.com>
Date:   Mon Jun 21 21:13:25 2010 -0600

    update several modules

commit c29e40fa686e7941af083e8b85f46204e1ee60c2
Author: rfay <randy@randyfay.com>
Date:   Sun Nov 22 12:26:50 2009 -0500

    Update several modules via drush

commit 3cb2d768891dfe6533557d2ebc7ea7e81ac23be4
Author: rfay <randy@randyfay.com>
Date:   Thu Nov 19 16:26:18 2009 -0500

    Initial commit

{% endhighlight %}


### git log --stat
summary of changset

#### git show
Defaults to show head
git show <commit>


# Writing

#### git add -p
**Description**: The git add -p option allows one to add `hunks` at a time per 
commit  
**Use case**: I use this for _every single commit_ I commit. It's helpful to 
separate logical pieces of a larger changeset into distinct commits to preserve
history and purpose better. You may also want to leave out some comments or
testing code but don't have time to do all the cleaning before you leave for the
day. With `git add -p` you can add just the lines you want, and deal with the 
rest later.

#### git commit -v
Show me what I'm commiting!

#### git filter-branch
remove password commit file
filter-branch -f --index-filter 'git rm --cached --ignore-unmatch password.txt' HEAD^^^^..HEAD 
commits are different, why?

#### git apply

### git stash 
**Description**: The git stash command is too oft-ignored in my o. 
**Use case**: Let's say as a developer, hypothetically, you're lazy. Instead of
 properly setting up a local environment to run over SSL like the production 
 site does, you decide to comment out the few lines in your .htaccess file that
 forces the redirect to https://. 
**Example**: The touring-cyclist hospitality network 


### git prune
Remove stale refs

#### git revert
Reverse a commit

### git commit --amend 
Rewrite history on the most recent commit

### git rebase - i
Safe + clean workflow, best of both worlds! 
Bitbucket = free private repos
Github / wherever else - collaboration point where others might see
Commit often, with messy explanatory messages and code. Always push to have
backup off of your laptop.
When code is tested, and working... step backwards with git rebase -i
make brand new commits using git add -p 
Document those well, commit appropriately
Checkout both projects
diff -rq to see if there are any file differences

### git cherry-pick
Apply a given commit to current branch

# Executing?


##### git bisect
Description: Find the bad commit
Use-case: Anytime something is breaking... often visually
Example:

#### git clean
Description: Never used it!
Use-case: Get rid of files 

### git checkout -b [<start point>]
Create a new branch and put HEAD to <start point> commit
without start point, you go from whatever branch you're on, could be 
a feature branch

### git reflog
See what happened to HEAD
Useful when rebasing a lot


{% highlight bash %}
MacBook-Pro:savas-site chris$ git branch -a
  58-add-photos
  add-blockquote-border
  ctools-block
* master
  natalias-artistic-vision-em-hmm-yeah-thats-right
  update-posts
  remotes/github/58-add-photos
  remotes/github/SAV-46-homepage-blogs
  remotes/github/a-man-a-plan-a-savas
  remotes/github/d8-theming-post
  remotes/github/drupal-planet
  remotes/github/mapping-in-d8-blab
  remotes/github/master
  remotes/github/update-posts
  remotes/kostajh/ctools-block
  remotes/kostajh/master
MacBook-Pro:savas-site chris$ git remote prune github 
Pruning github
URL: git@github.com:savaslabs/savaslabs.github.io.git
 * [pruned] github/a-man-a-plan-a-savas
 * [pruned] github/d8-theming-post
MacBook-Pro:savas-site chris$ git branch -a
  58-add-photos
  add-blockquote-border
  ctools-block
* master
  natalias-artistic-vision-em-hmm-yeah-thats-right
  update-posts
  remotes/github/58-add-photos
  remotes/github/SAV-46-homepage-blogs
  remotes/github/drupal-planet
  remotes/github/mapping-in-d8-blab
  remotes/github/master
  remotes/github/update-posts
  remotes/kostajh/ctools-block
  remotes/kostajh/master
{% endhighlight %}


