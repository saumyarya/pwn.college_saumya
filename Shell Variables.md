# Challenge 1 Printing Variables

The flag isn't printed by `/challenge/run`, but it has been placed in a shell variable named `FLAG`. Your job: print the contents of that variable from the shell.

## Solution:

- Shell variables are referenced with a leading `$`.
- The simplest way to print a variable is `echo $VAR`.
- If `FLAG` is already in your current shell environment, `echo $FLAG` prints it immediately.

#### Commands run: 

```sh
hacker@variables~printing-variables:~$ echo $FLAG
pwn.college{I5p_1W8tOD1grClohmDgiN_wbEz.QX3UTN0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{I5p_1W8tOD1grClohmDgiN_wbEz.QX3UTN0wyNwEzNzEzW}
```

### Notes:

- `echo $FLAG` is the simplest way to print the flag from the variable.

# Challenge 2 Setting Variables

Set the shell variable `PWN` to the value `COLLEGE`. Variable names and values are case-sensitive. Do not put spaces around the = (that makes the shell try to run a command instead of assigning).

## Solution:

- `NAME=value` assigns value to `NAME` in the current shell.
- The `$` is only used when reading a variable (expansion), not when assigning.
- No spaces around the =, `PWN = COLLEGE` is wrong; use `PWN=COLLEGE`.

#### Commands run: 

```sh
hacker@variables~setting-variables:~$ PWN=COLLEGE
You've set the PWN variable properly! As promised, here is the flag:
```

## Flag: 

```
pwn.college{8iNMmGHMxSfhrkhME_KPWNJpKAJ.QX5UTN0wyNwEzNzEzW}
```

### Notes:

- Common mistake: using `$PWN=COLLEGE`, `$` is only for reading, not assigning.

# Challenge 3 Multi-word Variables

Set the shell variable `PWN` to the value `COLLEGE YEAH`. Because the value contains a space, you must quote it so the shell treats the whole phrase as a single token.

## Solution:

- The shell splits words on whitespace; `PWN=COLLEGE YEAH` is parsed as `PWN=COLLEGE` followed by a command `YEAH`.
- To assign a value containing spaces you must quote the value: use double quotes ("...") or single quotes ('...') around `COLLEGE YEAH`.
- Use `echo $PWN` to verify the variable was set.

#### Commands run: 

```sh
hacker@variables~multi-word-variables:~$ PWN="COLLEGE YEAH"
You've set the PWN variable properly! As promised, here is the flag:
```

## Flag: 

```
pwn.college{sEdKFLsFtUFNH2YYENZcwmP_8PG.QXwYTN0wyNwEzNzEzW}
```

### Notes:

- Use double quotes ("...") when you want variable expansion inside the value (e.g., `X="hi $USER"`). Use single quotes ('...') to prevent expansion.

# Challenge 4 Exporting Variables

Invoke `/challenge/run` with the `PWN` variable exported and set to the value `COLLEGE`, and have the `COLLEGE` variable set to the value `PWN` in your shell but not exported (so `/challenge/`run` should not see `COLLEGE`).

## Solution:

- Variables are local to the current shell unless exported.
- To make a variable visible to a child process, export it (or put it in the command’s environment).
- The requirement: `PWN` must be exported with the literal value `COLLEGE`. `COLLEGE` must exist in the current shell with the literal value `PWN` but must not be exported.
- So: set `COLLEGE` (no export), then set & export `PWN` (assigning the literal `COLLEGE`), then run `/challenge/run`. The child process will inherit only exported variables (i.e., `PWN`) and not `COLLEGE`.

#### Commands run: 

```sh
hacker@variables~exporting-variables:~$ export PWN=COLLEGE
You've set the PWN variable to the proper value!
hacker@variables~exporting-variables:~$ COLLEGE=PWN
You've set the PWN variable to the proper value!
You've set the COLLEGE variable to the proper value!
hacker@variables~exporting-variables:~$ /challenge/run
CORRECT!
You have exported PWN=COLLEGE and set, but not exported, COLLEGE=PWN. Great 
job! Here is your flag:
```

## Flag: 

```
pwn.college{Uzj5VeF-fbdgl-CMhOWnQBsXZvL.QXyYTN0wyNwEzNzEzW}
```

### Notes:

- `COLLEGE=PWN` sets the variable `COLLEGE` to the string `PWN` in the current shell only (not exported).
- export `PWN=COLLEGE` sets `PWN` to the literal string `COLLEGE` and exports it for child processes. Don’t confuse this with export `PWN=$COLLEGE` (that would set `PWN` to the value of your `COLLEGE` variable).

# Challenge 5 Printing Exported Variables

The `FLAG` is stored as an exported environment variable. Use the `env` command (which lists exported environment variables) and find the `FLAG` value.

## Solution:

- `env` prints the current process’s environment (only exported variables).
- If `FLAG` is exported, `env` will show a line like `FLAG=....`

#### Commands run: 

```sh
hacker@variables~printing-exported-variables:~$ env
SHELL=/run/dojo/bin/bash
HOSTNAME=variables~printing-exported-variables
PWD=/home/hacker
MANPATH=/run/dojo/share/man:
DOJO_AUTH_TOKEN=aa01a9274b798b0bbca0aa4f3d96c35ff068b6059df3a7461087a3e4eb2e43e7
HOME=/home/hacker
LANG=C.UTF-8
FLAG=pwn.college{0MhcrwgddgzW-g_Qehemc-hAK_x.QX4UTN0wyNwEzNzEzW}
TERMINFO=/run/dojo/share/terminfo
TERM=xterm-256color
SHLVL=2
LC_CTYPE=C.UTF-8
SSL_CERT_FILE=/run/dojo/etc/ssl/certs/ca-bundle.crt
PATH=/run/challenge/bin:/run/dojo/bin:/root/.cargo/bin:/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
DEBIAN_FRONTEND=noninteractive
_=/run/dojo/bin/env
```

## Flag: 

```
pwn.college{0MhcrwgddgzW-g_Qehemc-hAK_x.QX4UTN0wyNwEzNzEzW}
```

### Notes:

- `env` only shows exported variables. If `FLAG` is not exported, `env` and `printenv` won’t show it, you’d need to be in the shell/process that has `FLAG` set or have it exported.

# Challenge 6 Storing Command Output

Read the stdout of `/challenge/run` directly into a shell variable named `PWN` using command substitution, so that `PWN` contains the flag. Use $(...) (preferred) or backticks `...` (older style).

## Solution:

- Command substitution `$(command)` runs command and replaces the expression with its stdout.
- Assign that expansion to a variable: `PWN=$( /challenge/run )`.
- Quote the variable when printing (`echo "$PWN"`) to preserve whitespace/newlines.

#### Commands run: 

```sh
hacker@variables~storing-command-output:~$ PWN=$(/challenge/run)
Congratulations! You have read the flag into the PWN variable. Now print it out 
and submit it!
hacker@variables~storing-command-output:~$ echo "$PWN"
```

## Flag: 

```
pwn.college{gYoxf0Lsj3YbV7ztAWS0oyCm2Q3.QX1cDN1wyNwEzNzEzW}
```

### Notes:

- Note: command substitution captures stdout only. If the program writes the flag to stderr you must redirect it (e.g., `$( /challenge/run 2>&1 )`).
- Also: command substitution strips trailing newlines; if you need to preserve multiple trailing newlines, use more advanced methods (not needed for most flags).

# Challenge 7 Reading Input

Use the `read` builtin to set the shell variable `PWN` to the value `COLLEGE`. `read` reads from standard input and stores the typed text into a variable.

## Solution:

- `read VARIABLE` waits for input from stdin and stores that line into `VARIABLE`.
- Use `-p` to show a prompt so it’s clear what to type interactively.
- Watch out: piping into `read` (e.g. `echo ... | read VAR`) runs read in a subshell in many shells, so the parent shell’s variable remains unset. Use a here-string or run read interactively to set the variable in the current shell.

#### Commands run: 

```sh
hacker@variables~reading-input:~$ read -p "INPUT= " PWN
INPUT= COLLEGE
You've set the PWN variable properly! As promised, here is the flag:
```

## Flag: 

```
pwn.college{ARjk-1o0ItiX4gn8eqAy9e-NH2-.QX4cTN0wyNwEzNzEzW}
```

### Notes:

- `read -r` prevents backslash escaping and is generally recommended.
- `read` reads a single line (stops at newline). Use `read -r` plus other flags for advanced behaviour.
- Piping into `read` can run it in a subshell (so the parent shell’s variable remains unset). Use a here-string (`<<<`) or interactive `read -p` to set variables in your current shell.

# Challenge 8 Reading Files

Read the contents of `/challenge/read_me` directly into the shell variable `PWN` using one command. Do not use `cat` (avoid the “Useless Use of Cat”). Use input redirection so `read` reads the file.

## Solution:

- `read` reads a line from stdin into a variable.
- Redirecting a file into `read` (`read VAR < file`) makes `read` read from that file instead of the keyboard.
- This is a single-command, shell-native replacement for `PWN=$(cat /challenge/read_me)`.

#### Commands run: 

```sh
hacker@variables~reading-files:~$ read PWN < /challenge/read_me
You've set the PWN variable properly! As promised, here is the flag:
```

## Flag: 

```
pwn.college{gn29yyLVFWgwTGbSssR0GkiBlQ_.QXwIDO0wyNwEzNzEzW}
```

### Notes:

- Avoid `cat /challenge/read_me | read PWN`, in many shells that runs `read` in a subshell so the parent shell’s `PWN` remains unset. Use the input-redirection form shown above.