# Challenge 1 The PATH Variable

`The /challenge/run` program will try to delete the flag with `rm`; if `rm` cannot be found in the child process’s PATH, the deletion fails and you get the flag. Your job is to run `/challenge/run` in an environment where the child’s PATH is empty (but don’t clobber your interactive shell globally).

## Solution:

- Run the challenge with an empty PATH for that one invocation: `PATH="" /challenge/run`.
- Reattach / check the output where the challenge promised the flag, the run should fail to delete the flag and the challenge will hand you the flag.

#### Commands run: 

```sh
hacker@path~the-path-variable:~$ PATH="" /challenge/run
Trying to remove /flag...
/challenge/run: line 4: rm: No such file or directory
The flag is still there! I might as well give it to you!
pwn.college{cmWR54rsFGhecXg5PFYsiJLSDJW.QX2cDM1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{cmWR54rsFGhecXg5PFYsiJLSDJW.QX2cDM1wyNwEzNzEzW}
```

### Notes:

- Do not `export PATH=""` globally in your interactive shell unless you know how to recover — that will make normal commands disappear.

# Challenge 2 Setting Path

`/challenge/run` invokes the `win` command by bare name, but `win` lives in `/challenge/more_commands/`, which isn’t in the shell `PATH`. Overwrite the child’s `PATH` so the shell will find `win` by name and `/challenge/run` succeeds.

## Solution:

- List the directory that contains win to confirm it exists: `ls /challenge/more_commands`.
- Confirm win is not currently runnable by bare name (optional check): `command -v win || echo "not found"`.
- Run the level while temporarily setting PATH `/challenge/more_commands`: `PATH=/challenge/more_commands`
- Run `win` followed by `/challenge/run` to get the flag.

#### Commands run: 

```sh
hacker@path~setting-path:~$ PATH=/challenge/more_commands/
hacker@path~setting-path:~$ win
It looks like 'win' was improperly launched. Don't launch it directly; it MUST 
be launched by /challenge/run!
hacker@path~setting-path:~$ /challenge/run
Invoking 'win'....
Congratulations! You properly set the flag and 'win' has launched!
pwn.college{AEHcvYQAW6C3Im6w48uz61b1-Ie.QX1cjM1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{AEHcvYQAW6C3Im6w48uz61b1-Ie.QX1cjM1wyNwEzNzEzW}
```

### Notes:

- do not assume `win` needs execute permission; if permission is wrong the above will still fail, but the challenge usually sets that up correctly.

# Challenge 3 Finding Commands

A `win` program has been placed somewhere in your `PATH`, but it won’t print the flag, instead the same directory contains a readable flag file. Use `which` to find the `win` binary’s location, then read the flag file from that directory.

## Solution:

- Find the full path to the win executable: `which win`.
- Get the directory that contains win.
- Read the flag using `cat /directory/flag`

#### Commands run: 

```sh
hacker@path~finding-commands:~$ which win
/challenge/paths/10350/win
hacker@path~finding-commands:~$ cat /challenge/paths/10350/flag
pwn.college{EETqThD-JgZhQjNC4BcH7yz2ck8.01NzEzNxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{EETqThD-JgZhQjNC4BcH7yz2ck8.01NzEzNxwyNwEzNzEzW}
```

### Notes:

- do not assume `win` prints the flag, the flag file is separate.

# Challenge 4 Adding Commands

Create a small `/home/hacker/mybin/win` program, put that directory into `PATH`, and let `/challenge/run` call `win` to reveal the flag; you can either call `/bin/cat` by absolute path, keep system dirs in `PATH`, or use the shell builtin `read` so you don’t need external binaries.

## Solution:

- The simplest `win` is a tiny script that outputs the flag via an absolute call like `/bin/cat /flag`, but when you run `win` as an ordinary user you might see `/bin/cat: /flag: Permission denied`, that’s expected because your user lacks permission; however `/challenge/run` runs as root and will succeed.
- If you prefer not to rely on root vs user differences, either prepend system dirs to `PATH` so `cat` is found by name, or implement `win` using the shell builtin `read` so it needs no external tools.
- I show three safe options (A/B/C) below; pick one, run the commands, then run `/challenge/run` to have the verifier invoke your `win` and print the flag.

#### Commands run: 

```sh
hacker@path~adding-commands:~$ mkdir -p /home/hacker/mybin
hacker@path~adding-commands:~$ cat > /home/hacker/mybin/win <<'EOF'
#!/bin/sh
/bin/cat /flag
EOF
hacker@path~adding-commands:~$ chmod +x /home/hacker/mybin/win
hacker@path~adding-commands:~$ export PATH=/home/hacker/mybin
hacker@path~adding-commands:~$ /challenge/run
Invoking 'win'....
pwn.college{MDMpegNpPznBscWiV31LmAih6do.QX2cjM1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{MDMpegNpPznBscWiV31LmAih6do.QX2cjM1wyNwEzNzEzW}
```

### Notes:

- If you overwrite `PATH` (`export PATH=/home/hacker/mybin`), any external commands your script needs must be absolute paths or present in that single dir.

# Challenge 5 Hijacking Commands

This level runs the system `rm` which would delete the flag; you can control `PATH`, so put a directory you own at the front and supply your own `rm` that preserves the flag (copy it somewhere safe) and then exits without removing anything.

## Solution:

- The shell searches directories in `PATH` left-to-right, so a directory you prepend will win and supply the `rm` the challenge runs.
- Write a tiny wrapper named `rm` that copies `/flag` to a safe location like `/tmp/flag.safe` and then exit 0 so nothing gets deleted.
- Use only the most basic commands (`mkdir`, `cat`/`cp`, `chmod`, `export`) so the solution is robust in restricted shells.
- Provide two reliable options: a safe debugging route that prepends a bin dir to `PATH` so you can test, and a compact one-shot that does it all in one line and runs the challenge.

#### Commands run: 

```sh
hacker@path~hijacking-commands:~$ mkdir -p /tmp/winbin
hacker@path~hijacking-commands:~$ cat > /tmp/winbin/rm <<'EOF'
#!/bin/sh
/bin/cat /flag
EOF
hacker@path~hijacking-commands:~$ chmod +x /tmp/winbin/rm
hacker@path~hijacking-commands:~$ export PATH=/tmp/winbin:$PATH
hacker@path~hijacking-commands:~$ /challenge/run
Trying to remove /flag...
Found 'rm' command at /tmp/winbin/rm. Executing!
pwn.college{AcpPB-OCoPJiBDVYvFJYEe_nO1R.QX3cjM1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{AcpPB-OCoPJiBDVYvFJYEe_nO1R.QX3cjM1wyNwEzNzEzW}
```

### Notes:

- Use `/tmp` or your home for the fake bin because those are writable in the dojo.