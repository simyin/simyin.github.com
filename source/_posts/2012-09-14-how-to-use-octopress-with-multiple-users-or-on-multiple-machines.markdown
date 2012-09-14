---
layout: post
title: "How to Use Octopress With Multiple Users or on Multiple Machines"
date: 2012-09-14 17:11
comments: true
categories: 
- Octopress
- Jekyll
published: true
author: Ivan Alagenchev
---

*This post assumes that the correct ssh public key is deployed on Github and that you have your private ssh key properly added to your .ssh folder.
We also assume that a [Jekyll](https://github.com/mojombo/jekyll/wiki) blog has already been created following the steps in our [previous post](http://simyin.github.com/blog/2012/09/12/setting-up-an-octopress-blog-on-github/)*
[This page](https://help.github.com/categories/56/articles) has good information about troubleshooting and generating ssh keys.
Test and troubleshoot, if needed your authentication following the steps [here](https://help.github.com/articles/error-permission-denied-publickey), before proceeding.

First, you will have to navigate to a folder where you would like to keep your blog posts and issue the following command:
 
``git clone git@github.com:simyin/simyin.github.com.git simyin_blog`` where you would replace `simyin_blog` with the name of your own blog and the github repository url with your own github url.

Now, navigate to the newly created folder: `cd simyin_blog/` and switch to the source branch of the blog repo: `git checkout source`.
We need to specify the username and email that git will be using for commits: 

>
    ~/simyin_blog$ git config user.name "Ivan Alagenchev" 
    ~/simyin_blog$ git config user.email "ivan@simy.in"

You can see your changes: `~/simyin_blog/$ vi .git/config`
 
Create the `_deploy folder` which will serve as the target folder for the generate rake task: `mkdir _deploy`.
Your folder structure should represent something similar to: 
       parent
          |
    my_blog_name
          |
      _deploy
      
Navigate to _deploy: `cd _deploy` and initialize a new git repository in the _deploy folder: 
>
    git init
    Initialized empty Git repository in /parent/simyin_blog/_deploy/.git/
    
Now execute: `git remote add origin git@github.com:simyin/simyin.github.com.git`. This adds a new remote branch, which will be named origin for short from now on and it will be pointing to the github repository, which you provided.
Next we'll do `git pull origin master` to pull all contents of the remote branch named origin (the one we added in the previous step) and associate it with a local branch named master.

We need to specify the username and email that git will be using for commits for this branch too: 

>
    ~/simyin_blog/_deploy$ git config user.name "Ivan Alagenchev" 
    ~/simyin_blog/_deploy$ git config user.email "ivan@simy.in"

You can see your changes: `~/simyin_blog/_deploy$ vi .git/config`

Now we can go to the parent directory `cd ..` and create posts as usual: ~/simyin_blog$ rake new_post["post title"], which we can now edit:

`~/simyin_blog$ gedit source/_posts/date-title.markdown`

Make sure to fill in the "author:" field in the [yaml front matter](https://github.com/mojombo/jekyll/wiki/yaml-front-matter) to associate posts with their authors. 
You can also make a draft post by specifying `published: false` in the yaml front matter and you can preview your post by running `rake preview` from the command line and navigating to `localhost:4000`.

Rake preview has a file listener, which detects any changes that are made to files, so you don't have to rerun the command every time you make changes to your post. Refreshing your browser window will display the new changes.

Once you are finished with your blog post, you can generate the post using `rake generate` and deploy using `rake deploy`, or combine the steps with `rake gen_deploy`.

Do not forget to commit your post to github after you make your changes: 
>
    git add .
    git commit -m "message"
    git push
    
This will make sure that all your posts are nicely backed up - one of the biggest advantages of using Github and Jekyll.



