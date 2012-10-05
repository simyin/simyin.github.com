---
layout: post
title: "Gnome Keyring Bug Mismanages SSH Keys"
date: 2012-10-05 13:58
comments: true
categories: 
- ssh
- github
- gnome
- seahorse
- debian
author: Ivan Alagenchev
---

I use several ssh keys to connect to git on the same machine. I have my personal github account, the company github account and also a separate git account for an internal company repository. 
We manage all account authentication via ssh RSA keys and naturally they all reside in my ~/.ssh folder. Today, I was trying to make a push to github from an account that I hadn't used for a while
and github reported a "Permission Denied" error. I noticed that git was trying to push with the wrong user account. I thought to myself "Not a biggie, I'll just temporarily remove they keys that I don't need. That's usually done executing `ssh-add -d \path\to\key` to remove the key one doesn't want to use anymore. However, I was completely caught by surprise, `ssh-add -l` would still show the same keys listed without any changes. 

After a bit of searching, I found the following [bug reported on debian's bug tracker](http://bugs.debian.org/cgi-bin/bugreport.cgi?bug=472477). Apparently this is a known, though not actively worked on, bug in gnome-keyring. To work around the issue move all your keys from `~\.ssh ` to another folder and open seahorse (keyring management tool) and remove all your unneeded keys from it. Your `ssh-add -l` should report the correct keys from now on and your `git push` should succeed. 
