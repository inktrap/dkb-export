## TODO

 - test and audit the tools individually
 - write a wrapper as detailed below (see: Setup)


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


## Rationale

Currently I am including the projects as subrepositories from their authors
(hoping that they will fix bugs and develop new features). If a project is
abandoned I am including my fork (forked with the intention to maintain it) as
a subrepository.

That way I can audit the tools and check the changes, which leads to (hopfully
well) audited and maintained, therefore trustable tools.


## Setup

An overview of the financial data tools:

 - ``[dkbweb] and [dkb-visa] --pull/export--> data (qif) --import--> data in [gnucash]``
 - ``[autobudget] --budget--> budgeted data in [gnucash]``
 - financial analysis with gnucash


The ``--import-->`` step should be performed if GnuCash is started (write
a wrapper that acts as a controller):

 - call dkb*-scripts
 - ask for the password
 - call autobudget
 - call gnucash
 - for all steps: check/report output/results


I don't feel comfortable to automate this with ``daemontools`` or ``(ana)cron``
or ``runwhen``:

 - I would like direct feedback if something goes wrong.
 - I won't include my password in a script in cleartext.
 - I don't need regular checks, only if I start GnuCash.

