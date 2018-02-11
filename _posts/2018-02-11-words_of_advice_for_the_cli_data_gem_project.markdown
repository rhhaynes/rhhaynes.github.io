---
layout: post
title:      "Words of Advice for the CLI Data Gem Project"
date:       2018-02-11 14:19:27 -0500
permalink:  words_of_advice_for_the_cli_data_gem_project
---

Upon reading the requirements for the first portfolio project, I was excited for the challenge.  While completing labs is fun, I knew nothing would compare to the satisfaction of writing a working program from start to finish.  Aside from setting my idea into action though, I realized one of my favorite parts of the experience was actually figuring out how to get the gem running as expected for publishing and installation .  Thus I've decided to share three pieces of advice I wish I'd known (explicitly) prior to beginning.

## Getting started is hard... or is it?
I'm not going to lie, prior to this project I had taken the setup and helper files for granted.  Sure I knew what went into a Gemfile and why we needed to utilize an environments file, but the labs focus almost entirely on coding in the \*.rb files.  As such I found the blank terminal screen to be extremely daunting.
* Where do I begin?
* What files and directories should I create?
* Is there a certain format or directory layout required for creating and publishing a gem?
* How do I specify the version number?
* What all goes in the \*.gemspec file?
These are just a few of the questions that overwhelmed me at the beginning.  I knew if I had to search around online and look back through earlier labs I could figure them out, however it seemed like a considerable amount of up-front work at a time when all I really wanted to do was start coding.

The solution to my dilemma:  `bundle gem <gem-name>`.

This nifty command does all of the setup and dirty work for you, leaving you to focus on the good stuff.

## Write the code you wish you had. --Avi Flombaum
If you've watched any of Avi's sample videos, I assume you've heard him use this phrase time and time again.  In the labs though I was never really sure how it applied, since typically there was only one task at hand and the code you wish you had was exactly the code you needed to write!  However during the project it helps immensely to think about things this way; remember you're starting with nothing.  At numerous stick points, especially object interfaces, I would simply write a comment like the one shown below and move on, faking the code by hard-coding a return if it was absolutely necessary.

`# code that accepts <something> and returns <something-else>`

This form of *coding procrastination* allowed me to move forward without worrying at the moment about how such code would work.  What's more, in many cases the code I ended up writing was completely different than what I had initially intended, but that's the point.  There's no need to worry about the inner workings of code that you don't actually need yet.

## When in doubt, check the gemspec
Once I had my gem working as expected by running the bin/executable, I was disppointed to find that I could not call it from the command line.  After a few Google searches and trying various tweaks, I was able to narrow down the issue to a problem in my \*.gemspec file.  Although this file gets generated automatically by running `bundle gem <gem-name>`, there are a few modifications that must be made before the gem works from the command line and can be released.  Some of the changes are obvious and have lines that begin with `TODO`, however in my case the lines that required attention are shown below.

`spec.bindir = "exe"`

`spec.executables   = spec.files.grep(%r{^exe/}) { |f| File.basename(f) }`

Fortunately the fix was simple.  To get my gem working from the command line I simply needed to remove the `spec.bindir` line and edit the other to read `spec.executables = ["<gem-name>"]`.

As a reminder you also need to add gem dependencies in this file using the command below.

`spec.add_development_dependency`

So in closing, if you're having problems that do not appear to be code related, check the \*.gemspec file!

## Shameless plug
If interested, check out my gem by typing `gem install last_tweet` in the terminal (to install the gem) and then `last_tweet` to run it!  Without going into detail, the gem will return the most recent tweet from all specified Twitter handles.  Enjoy!
