Test-C
======

* Ever wanted to write, compile and run a snippet of C code small
  enough that making a new file and writing the boiler plates puts you
  off?
* Tired of doing M-x compile with the same command?
* Accumulating small useless variations of "test.c" in your home
  directory?

Here comes `test-c`!

Installation
============

Place `test-c.el` somewhere on your load-path and load it.

Usage
=====

Call M-x `test-c` to open a temporary `*test-c*` buffer. It is
prefilled with a skeleton C program (customized through
`test-c-default-code`) which is then compiled and run.

Every following call to `test-c` will compile and run the program
and show its ouput in the minibar.

You can customize the compilation and run commands from the source
itself using special definitions lines (very similar to Emacs file
local variables in concept). Those lines must be of the form:

    /*= var: value =*/

The `compile` and `run` variable are the one used respectively for
compiling and running the file. You can refer to other variable from
these variables using the `$var` syntax, similar to the shell. If you
refer to a variable which has not been defined it will be passed as is
to the shell, who might expand them (i.e. you can use shell/env
variables too).

The default value of `compile` and `run` inserted with the initial
skeleton can be customized via the `test-c-default-compile-command`
and `test-c-default-run-command` variables.

`$exe` and `$src` are special variabled defined by test-c that expands
to respectively the temporary executable filename and the temporary
source file name.

You can save the file and keep using Test-C afterwards.

Sample configuration
====================

    ;; bind to win+m
    (global-set-key (kbd "s-m") 'test-c)
     
    (eval-after-load 'test-c
      '(progn
         ;; update the default command to use clang
         (setq test-c-default-compile-command "clang -Wall $src -o $exe")
     
         ;; print error code after running
         (setq test-c-default-run-command "$exe ; echo $?")
     
         ;; use simpler skeleton
         (setq test-c-default-code "int main (void) {\nreturn 0;\n}\n")))
