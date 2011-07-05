git-scripts
===========

This is my collection of git scripts I use to make my day-to-day interaction with git just a bit slicker.
There could definitely be bugs; I'd be happy to accept any patches or enhancements. Right now these are
quick'n'dirty scripts that get the job done and nothing more.

I've included some descriptions of the scripts below; you can also use the `--help` flag to get more info.

Descriptions
------------

* `git sync`: Shorthand for rebasing master onto your dev/feature branch and then fast-forwarding master with
  the changes. Takes the same arguments as the rebase.

Installation
------------

Just clone this repo somewhere and add it to your `$PATH`.

TODO
----

These work just fine for me, but I suppose I could package them as a gem or something to make them easier to
install.
