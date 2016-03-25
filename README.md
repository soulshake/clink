# clink: containerize all the things

Warning: This is extremely beta.

## Installation

    $ git clone https://github.com/soulshake/clink.git

Add to your profile:

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

If no matching executable is found in your path, your shell will look for a function named `command_not_found_handle` (in the case of bash) or `command_not_found_handler` (in the case of zsh). By sourcing `clink` in your profile, you are defining these functions, which will take over. `clink` looks for Docker images on your system with a matching tag and attempts to run them for you.

Missing commands are handled a bit differently depending on your shell.

### zsh

#### command_not_found_handler

From the zsh man page:

    If no external command is found but a function command_not_found_handler exists
    the shell executes this function with all command line arguments. The function
    should return status zero if it successfully handled the command, or non-zero 
    status if it failed. In the latter case the standard handling is applied:
    `command not found' is printed to standard error and the shell exits with status 127.
    Note that the handler is executed in a subshell forked to execute an external command,
    hence changes to directories, shell parameters, etc. have no effect on the main shell.

### bash

#### command_not_found_handle


From the bash man page:

    If the search is unsuccessful, the shell searches for a defined shell function
    named command_not_found_handle. If that function exists, it is invoked with the
    original command and the original command's arguments as its arguments, and the
    function's exit status becomes the exit status of the shell. If that function is
    not defined, the shell prints an error message and returns an exit status of 127.

For more information, see:

    man --pager='less -p command_not_found' bash

   Function names and definitions may be listed with the -f option to the declare or typeset builtin commands.
   The -F option to declare or typeset will list the function names only (and optionally the source  file  and
   line  number,  if the extdebug shell option is enabled).  Functions may be exported so that subshells auto‐
   matically have them defined with the -f option to the export builtin.  A function definition may be deleted
   using  the  -f option to the unset builtin.  Note that shell functions and variables with the same name may
   result in multiple identically-named entries in the environment  passed  to  the  shell's  children.   Care
   should be taken in cases where this may cause a problem.

### `which` integration

Obviously, Docker images don't appear in your system path:

    $ /bin/which ubuntu
    < no output >

But when sourced, this program redefines (actually, wraps) the `which` command so that tagged images show up when a user runs `which IMAGE` on their system:

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

### add to $PATH

It would be nice if image names could be autocompleted and/or found with any of `which`, `type -a` or `declare -f`, instead of redefining which.


## Acknowledgements

Inspired by [jfrazelle](https://github.com/jfrazelle/dockerfiles/blob/master/bashrc).
