# Challenge 1 Learning From Documentation

This level is about reading (or being told) the program's documentation and supplying the correct argument. The program `/challenge/challenge` expects the `--giveflag` argument; invoking it without that argument won't produce the flag.


```sh
hacker@man~learning-from-documentation:~$ /challenge/challenge --giveflag
Correct argument! Here is your flag:
```

## Solution:

- The mini-doc tells us the program requires the `--giveflag` argument.
- To get the flag, run `/challenge/challenge --giveflag`.
- Capture the output and copy the flag exactly.

## Flag: 

```
pwn.college{UGd8vy9yVQuZAhS45q5N0wuvLws.QX0ITO0wyNwEzNzEzW}
```

### Notes:

- Always read the program documentation or prompt for required arguments.
- ```--giveflag``` is the exact argument required here; typos or missing `--` will fail.

# Challenge 2 Learning Complex Usage

This level teaches that some command-line arguments accept their own arguments. The program `/challenge/challenge` supports `--printfile <path>`, which prints the file at `<path>`.


```sh
hacker@man~learning-complex-usage:~$ /challenge/challenge --printfile /flag
Correct argument! Here is the /flag file:
```

## Solution:

- The documentation states: `/challenge/challenge --printfile <path>` prints the contents of `<path>`.
- To get the flag, call the program with `/flag` as the argument to `--printfile`.
- So, run: `/challenge/challenge --printfile /flag` and copy the printed flag.

## Flag: 

```
pwn.college{U-ES8FGhZL3ic_ycbwPYx2j6Lhr.QX1ITO0wyNwEzNzEzW}
```

### Notes:

- Some commands take arguments for their arguments (like ```find -name <pattern>```). This is one of those cases.

# Challenge 3 Reading Manuals

This level teaches how to use `man` (the manual) to discover options for a program. The `challenge` program includes a secret option that, when invoked, prints the flag. We must read the manpage to learn that option.


```sh
hacker@man~reading-manuals:~$ man challenge
hacker@man~reading-manuals:~$ /challenge/challenge --cmhzyb 869
Correct usage! Your flag:
```

## Solution:

- I opened the manual for `challenge` using `man challenge` and searched the page for terms like “flag” or “secret”.
- The manual documented a special option (`--cmhzyb NUM` prints flag if NUM is 869) that triggers the flag output.
- I executed `/challenge/challenge` with that option and copied the printed flag.

## Flag: 

```
pwn.college{8ZUcYmh69MZzBybfFxo4kB6bt9N.QX0EDO0wyNwEzNzEzW}
```

### Notes:

- `man` is the place to learn a command’s options and usage.
- `man` do not use file location rather just the command to give correct output.

# Challenge 4 Searching Manuals

This level teaches how to use `man` (the manual) to find command options. The `challenge` program documents a secret option that will print the flag when used.

```sh
hacker@man~searching-manuals:~$ man challenge
hacker@man~searching-manuals:~$ /challenge/challenge --dy
Initializing...
Correct usage! Your flag:
```

## Solution:

- Opened the manual with `man challenge` and searched for keywords like `flag`, `give`, `print`, or `secret`.
- Found the secret option (`--dy`), which per the man page triggers the flag output.
- Executed `/challenge/challenge --dy` and copied the printed flag.

## Flag: 

```
pwn.college{glJOIV0q-kODDLzrRJA5IcMZlV2.QX1EDO0wyNwEzNzEzW}
```

### Notes:

- Use `/` inside man to search, `n` to go to next match, and `q` to quit.
- The man page is the authoritative source for options and usage.

# Challenge 5 Searching For Manuals

This level hides the manpage for `/challenge/challenge` under a randomized name. man man teaches you advanced usage of the man command itself, and you must use this knowledge to figure out how to search for the hidden manpage that will tell you how to use /challenge/challenge

```sh
hacker@man~searching-for-manuals:~$ man man
hacker@man~searching-for-manuals:~$ man -k challenge
tvvxumpgrn (1)       - print the flag!
hacker@man~searching-for-manuals:~$ man tvvxumpgrn
hacker@man~searching-for-manuals:~$ /challenge/challenge --tvvxum 822
Correct usage! Your flag:
```

## Solution:

- Opened the manual with `man man` and found `man -k` for searching for keywords.
- Used `man -k challenge` to search the manpage database for entries related to the challenge.
- Opened the relevant manpage with `man tvvxumpgrn` and searched inside (`/flag`, `/give`, etc.) to find the documented secret option.
- Executed `/challenge/challenge` with the discovered option to print the flag.

## Flag: 

```
pwn.college{8tvGvIGQASH2PxJMumpYg2r_no2.QX2EDO0wyNwEzNzEzW}
```

### Notes:

- Use ```man -k``` first: that lists manpage entries and short descriptions.
- Exact spelling matters: copy the option exactly from the manpage (e.g., ```--giveflag``` vs ```--give-flag``` are different).

# Challenge 6 Helpful Programs

Some programs don’t ship a full manpage, but provide quick usage help when invoked with `--help` (or `-h`). This level teaches checking a program’s builtin help to learn how to run it, and thus how to get the flag.

```sh
hacker@man~helpful-programs:~$ /challenge/challenge --help
usage: a challenge to make you ask for help [-h] [--fortune] [-v] [-g GIVE_THE_FLAG] [-p]

optional arguments:
  -h, --help            show this help message and exit
  --fortune             read your fortune
  -v, --version         get the version number
  -g GIVE_THE_FLAG, --give-the-flag GIVE_THE_FLAG
                        get the flag, if given the correct value
  -p, --print-value     print the value that will cause the -g option to give you the flag

hacker@man~helpful-programs:~$ /challenge/challenge --print-value
The secret value is: 769
hacker@man~helpful-programs:~$ /challenge/challenge -g 769
Correct usage! Your flag:
```

## Solution:

- Many programs support `--help` or `-h` to print a short usage summary and available options.
- The challenge program may print the flag if run with a specific help-style argument.
- The plan: run the program with `--help` (and fallback to `-h`), read the usage text, then run the program with the documented flag to get the flag output.
- The command asked to use was `-p` to get the secret number and the command to get the flag used was `-g GIVE_THE_FLAG`.

## Flag: 

```
pwn.college{Y7U6W95uvjrSx7BoFHpH2xZtGni.QX3IDO0wyNwEzNzEzW}
```

### Notes:

- `--help` is the fastest way to discover program options when manpages are absent.
- Common fallbacks: `-h`, `help`, or sometimes `/?` on Windows-style tools.

# Challenge 7 Help For Builtins

This level uses a shell **builtin** rather than an external program. Builtins are documented via the shell `help` builtin. We used `help` to find the secret option that makes the builtin print the flag.

```sh
hacker@man~help-for-builtins:~$ help challenge
challenge: challenge [--fortune] [--version] [--secret SECRET]
    This builtin command will read you the flag, given the right arguments!
    
    Options:
      --fortune         display a fortune
      --version         display the version
      --secret VALUE    prints the flag, if VALUE is correct

    You must be sure to provide the right value to --secret. That value
    is "8RL7jfQK".
hacker@man~help-for-builtins:~$ challenge --secret "8RL7jfQK" 
Correct! Here is your flag!
```

## Solution:

- The challenge binary is a shell builtin (not an external program), so it won’t have a manpage or a `/path` invocation.
- Use `help challenge` to read its documentation. Inside the help output, search for terms like `flag`, `give`, or `secret` to find the option that prints the flag.
- Invoke the builtin with that option (no leading `/`, just `challenge <option>`), and copy the printed `pwn.college{...}` string.

## Flag: 

```
pwn.college{8RL7jfQKuW0UTgHwSUOCzgB7aze.QX0ETO0wyNwEzNzEzW}
```

### Notes:

- Builtins run inside the shell, invoke them by name (challenge ...) not by path.
- `help` is the canonical way to view builtin docs; `man` won’t help for builtins.