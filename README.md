git-scripts
===========

This is my collection of git scripts I use to make my day-to-day interaction with git just a bit slicker.
There could definitely be bugs; I'd be happy to accept any patches or enhancements. Right now these are
quick'n'dirty scripts that get the job done and nothing more.

I've included some descriptions of the scripts below; you can also use the `--help` flag to get more info.

git-sync
--------

`git sync` is a shorthand for rebasing master onto your dev/feature branch and then fast-forwarding master
with the changes. It takes the same arguments as the rebase.

git-filename
------------

This is a handy script for printing filenames of modified and untracked files in your repository. It works
well in combination with other git commands, like `git add`, as well as with system commands such as `rm` or
`grep`. See `git filename -h` for a full list of commands. I will include some example usages here.

    $ git filename --staged --newlines # Print each path to a staged change on a separate line
    $ g f -sn                          # Same as above, given the proper bash and git aliases
    $ rm `g f -t`                      # Delete untracked files (NOTE: you should use git-clean for this instead)
    $ g add `g f /src/.*\.c`           # git add all C files in /src/ (you can specify arbitrary ruby regexes)
    $ g reset HEAD `g f --any \.h$ ~$` # Unstage files ending in .h or ~
    $ vim `g f -m`                     # Open all merge conflicts in vim

Installation
------------

Just clone this repo somewhere and add it to your `$PATH`.

TODO
----

These work just fine for me, but I suppose I could package them as a gem or something to make them easier to
install.
