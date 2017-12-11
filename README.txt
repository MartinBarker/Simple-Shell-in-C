Martin Barker 2017
This program executes a simple shell in c
The shell can run background proccesses, command line arguments, and perform I/O redirection.
The shell uses fork(), exec(), and waitpid() to execute commands.
The shell supports three built in commands: exit, cd, and status. As well as regular commands like 'ls', 'sleep', 'pwd', 'cd', etc...

A CTRL-C command from the keyboard will send a SIGINT signal to your parent process and all children at the same time.

A CTRL-Z command from the keyboard will send a SIGTSTP signal to your shell. When this signal is received, your shell will display an informative message (see below) and then enter a state where new commands can no longer be run in the background.


To compile this program, use:

gcc -o smallsh smallsh.c

and run it with

./smallsh

the following is an example run of the code showing the different shell commands runable:

$ smallsh
: ls
junk   smallsh    smallsh.c
: ls > junk
: status
exit value 0
: cat junk
junk
smallsh
smallsh.c
: wc < junk > junk2
: wc < junk
       3       3      23
: test -f badfile
: status
exit value 1
: wc < badfile
cannot open badfile for input
: status
exit value 1
: badfile
badfile: no such file or directory
: sleep 5
^Cterminated by signal 2
: status &
terminated by signal 2
: sleep 15 &
background pid is 4923
: ps
  PID TTY          TIME CMD
 4923 pts/0    00:00:00 sleep
 4564 pts/0    00:00:03 bash
 4867 pts/0    00:01:32 smallsh
 4927 pts/0    00:00:00 ps
:
: # that was a blank command line, this is a comment line
:
background pid 4923 is done: exit value 0
: # the background sleep finally finished
: sleep 30 &
background pid is 4941
: kill -15 4941
background pid 4941 is done: terminated by signal 15
: pwd
/nfs/stak/faculty/b/brewsteb/CS344/prog3
: cd
: pwd
/nfs/stak/faculty/b/brewsteb
: cd CS344
: pwd
/nfs/stak/faculty/b/brewsteb/CS344
: echo 4867
4867
: echo $$
4867
: ^C
: ^Z
Entering foreground-only mode (& is now ignored)
: date
 Mon Jan  2 11:24:33 PST 2017
: sleep 5 &
: date
 Mon Jan  2 11:24:38 PST 2017
: ^Z
Exiting foreground-only mode
: date
 Mon Jan  2 11:24:39 PST 2017
: sleep 5 &
background pid is 4963
: date
 Mon Jan 2 11:24:39 PST 2017
: exit $
