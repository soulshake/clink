# clink: containerize all the things

## Installation

    $ curl get.clink.io | sh

## Usage

    $ clink list

## How it works

When a command name is sent to the shell, the shell looks in $PATH for matches.
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

## To do

### Install script

Need to make the script for get.clink.io (or whatever).

### add to $PATH

#### Integrate with `which`?

After running `hash -p ~/git/clink/clink clink`, the command `clink` works, but cannot be autocompleted and is not found with any of `which`, `type -a` or `declare -f`.

# user or $(whoami) ?


