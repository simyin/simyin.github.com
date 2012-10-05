---
layout: post
title: "Grails Deletes plugin-classes on grails clean"
date: 2012-10-05 13:03
comments: true
categories: 
- Grails
- Errors
author: Ivan Alagenchev
---
Today [Grails](grails.org) gave me the following error: 
>
    Description Resource    Path    Location    Type
    The container 'Grails Dependencies' references non existing library '/home/alagenchev/.grails/2.1.0/projects/com.vpundit.www/plugin-classes'
    The project cannot be built until build path errors are resolved    com.vpundit.www     Unknown Java Problem

I didn't know what was causing the problem at first, but I went to the folder in the error message and created the plugin-classes folder, 
`mkdir /home/alagenchev/.grails/2.1.0/projects/com.vpundit.www/plugin-classes`. The problem disappeared, however it was back again when I did `grails clean`.
Obviously `grails clean` was deleting my plugin-classes folder.

Here is how to fix the problem: 

Navigate to your `GRAILS_HOME` directory and edit the `GRAILS_HOME/scripts/_GrailsClean.groovy` script. Comment the following line: 

>
    ant.delete(dir:pluginClassesDirPath, failonerror:false)

Voila! Now you just need to recreate the plugin-classes folder again and you are set.

