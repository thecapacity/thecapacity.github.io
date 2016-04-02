---
id: 373
title: Postgresql Mac Installation
date: 2010-11-25T09:27:25+00:00
author: jay
layout: post
guid: http://blog.thecapacity.org/?p=373
permalink: /2010/11/25/postgresql-mac-installation/
categories:
  - frustration
---
Just a quick note for Thanksgiving.

I’m doing some experimentation and thought, given my frustrations with Sun and Oracle that I’d check out PostgreSQL instead of mysql (my typical DB of choice).

Unfortunately, the <a>Mac installer</a> threw some issues, stating:

> “The database cluster initialization failed.”

If you’re interested you can check out:

> sudo less /tmp/bitrock_installer.log

And at the end you’ll probably see a failure such as:

> “su: no directory”

I tried running the command manually, but got the same error and realized that’s because I set my user to “postgres” but that user doesn’t exist.

The installer tells you it will create the user, but apparently doesn’t. So if you run the initcluster.sh script as your normal user (or set the installer to use a user that exists) you should be ok.

Happy Turkey Day and hope that helps someone!!
