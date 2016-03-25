# clink: The magic containerizer


- comes as a single file, that you can download from a given easy URL
- let's call it CRUN for now


In the following examples, we have a few images, e.g.:
- imagemagick
- bin/traintcl
- soulshake/wopr

We assume that our username (as indicated by `$USER`) is `soulshake`.

We want to execute those images from the shell, like normal binaries.

Those images should be executed with a number of flags (see `bashrc`
from Jesse's `dockerfiles` repo for details).

In the following examples, we want to achieve this:

    docker run FLAGS imagemagick convert blah.jpg blah.png

CRUN supports multiple forms of invocation.

## Direct execution

Doesn't require any special step:

    CRUN imagemagick convert blah.jpg blah.png


## Symlinked execution

A symbolic link is created, with the name of the container that we
want to execute:

    ln -s CRUN imagemagick
    imagemagick convert blah.jpg blah.png


## Bash missing command hook

If CRUN is *sourced* from a bash shell, it adds the missing
command hook so that invoking the container is possible directly:

    imagemagick convert blah.jpg blah.png

If detecting sourcing is too complicated, we could imagine the
following fallback:

    eval $(CRUN --install-bash-hook)


## Support for other shells

I don't know if zsh, fish, etc. could suppor this. TBD.


## Shell function creation

Alternate option for shells which don't have a missing command hook:

    eval $(CRUN --install-shell-func imagemagick)

... Will add a shell function so that `imagemagick`
invokes the container.

Also, maybe:

    eval $(CRUN --install-all-shell-funcs)


## Image naming

When asked to execute `foo`, CRUN tries to execute (in this order):
- an image called `$USER/foo`
- an image called `bin/foo`
- an image called `foo`


