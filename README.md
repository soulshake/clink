# clink: containerize all the things

Warning: This is **extremely beta.** Use at your own risk.

## What is it?

TL;DR: When you type a command that's not installed on your system, `clink` looks for
and launches the appropriate Docker container.

This is useful if you often run desktop applications in containers, and are
sick of creating aliases for long and unwieldy `docker run` commands.

`clink` takes advantage of the `command not found` shell hook. When you run a
command that's not installed, it looks for an appropriate match among your local
Docker images, and tries to start a container, passing it things like the following:

* Devices:

    * Lots of them

* Environment variables:

    * All of them.

* Volumes, such as:

    * `/etc/passwd`
    * `/etc/group`
    * `/etc/localtime`
    * and perhaps most importantly, **your current working directory.** If this
    is your homedir, your profile setup may do funny things in the container. If
    you are in `/`, well, you get the idea.


## Installation

    $ git clone https://github.com/soulshake/clink.git

Add it to your profile:

    source PATH_TO_CLONED_REPO/clink

Bash is supported. Zsh *might* work.

## Usage

Build a docker image with the name of the command you want to run:

    $ docker build -t COMMAND_NAME .

Then just run COMMAND_NAME as usual and `clink` will run a Docker container of the corresponding image.

Like all sufficiently advanced technology, this is a dirty hack under no circumstances to be confused with magic.

## How it works

When a command name is sent to the shell, the shell looks in $PATH for matches.

If an executable is found in your path, your shell will run it like it normally does.

Missing commands are handled a bit differently depending on your shell.

If no matching executable is found in your path, your shell will look for a function named `command_not_found_handle` (in the case of bash) or `command_not_found_handler` (in the case of zsh). By sourcing `clink` in your profile, you are defining these functions, which will take over. `clink` looks for Docker images on your system with a matching tag and attempts to run them for you.


### `which` integration

Obviously, Docker images don't appear in your system $PATH:

    $ /bin/which ubuntu
    < no output >

But when sourced, this program redefines the `which` command so that tagged images show up when a user runs `which IMAGE` on their system:

    $ which ubuntu
    bash: type: ubuntu: not found
    REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
    ubuntu              14.04               1c9b046c2850        11 weeks ago        187.9 MB
    ubuntu              latest              1c9b046c2850        11 weeks ago        187.9 MB

    $ ubuntu
    Found image; running it
    aj@27872ecd864e:~/git/clink$


## To do

### Install script

Need to make the script for get.clink.tld (or whatever).

### autocompletion / add to $PATH

It would be nice if image names could be autocompleted and/or found with any of `which`, `type -a` or `declare -f`, instead of redefining which.


## Acknowledgements

Inspired by [@jfrazelle / @jpetazzo bashrc](https://github.com/jfrazelle/dockerfiles/blob/master/bashrc).
