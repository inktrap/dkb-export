# My nerdy and open source approach on financial data management without reinventing the wheel

My goal is to track and manage my finances as automated as possible using an
envelope system. I won't use external servers or companies or closed source
products, instead I will use the tools published here and GnuCash.


## TODO

 - test and audit the tools individually
 - write a wrapper as detailed below (see: Setup)
 - have a nicer and neater gnucash, see ``gtk``


## Overview

This repository has the purpose to track several tools that I use to interact
with DKB and to [manage my
money](https://martin.kleppmann.com/2011/03/07/accounting-for-computer-scientists.html).
These are:

 - ``dkbweb`` which is used to get info from my banking account. The project
 has quite some history: it was written a long time ago, then abandoned, then
 automatically exported from google code (multiple times) and then forked by
 me.

 - ``dkb-visa`` from hoffie is quite nice and works -- no need to fork it and
 I trust him to keep his version up to date.

 - ``gnucash_autobudget``  is a python script that creates GnuCash entries
 according to an [envelope
 system](https://en.wikipedia.org/wiki/Envelope_system). Currently it is not
 working, but maybe I'll have a look at it. Strange that the branch is named
 starting-out and not merged to master. But hey, there are tests, soo, I'll
 see if I am going to fork it.

 - ``gtk`` contains a custom GnuCash stylesheet [from
 here](https://github.com/Gnucash/gnucash/blob/master/doc/gtkrc-2.0.gnucash).
 Theming GTK+ applications is a whole new topic by itself. Maybe [change the
 gtk
 theme](https://wiki.archlinux.org/index.php/GTK%2B#Basic_theme_configuration)
 or [tweak the theme with
 css](https://blogs.gnome.org/mclasen/2014/05/06/tweaking-a-the-gtk-theme-using-css/)
 with the help of [gtkparasite](https://github.com/chipx86/gtkparasite)?


## Rationale

Currently I am including the projects as subrepositories from their authors
(hoping that they will fix bugs and develop new features). If a project is
abandoned I am including my fork (forked with the intention to maintain it) as
a subrepository.

That way I can audit the tools and check the changes, which leads to (hopfully
well) audited and maintained, therefore trustable tools.


## Setup

An overview of the financial data tools and their function:

 - from desktop ``[dkbweb] and [dkb-visa] --pull/export--> data (qif) --import--> in [gnucash]``
 - from mobile ``[GnuCashApp] --pull/export--> data --import/fiximport --> in [gnucash]``
 - budgeting ``[autobudget] --budget--> budgeted data in [gnucash]``
 - financial analysis with gnucash


The ``--import-->`` step should be performed if GnuCash is started (write
a wrapper that acts as a controller):

 - call dkb*-scripts
 - ask for the password
 - export data from mobile
 - import data from mobile and files
 - call autobudget
 - call gnucash
 - for all steps: check/report output/results


I don't feel comfortable to automate this with ``daemontools`` or ``(ana)cron``
or ``runwhen``:

 - I would like direct feedback if something goes wrong.
 - I won't include my password in a script in cleartext.
 - I don't need regular checks, only if I start GnuCash.

It would be nice to use the [Python
Bindings](https://wiki.gnucash.org/wiki/Python_Bindings) (see ``hjacobs``) and
develop a standalone script.

