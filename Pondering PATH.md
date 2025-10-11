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

`screen` sessions can hold multiple windows (like tabs). For this challenge you attach to a session, switch between windows, and find the flag in a window.

## Solution:

- Attach with `screen -r`.
- Use either `Ctrl-A n`, `Ctrl-A p`, or `Ctrl-A 1` (or the menu via `Ctrl-A "`) to move to window 1.
- Read the flag (it may already be printed). If needed and allowed, run `cat /flag` inside that window.
- When done, detach with `Ctrl-A d` or exit the session with `exit`.

#### Commands run: 

```sh
hacker@terminal-multiplexing~switching-windows:~$ screen -r
 cat <<MSG
Welcome to the window switching challenge!
You are currently in window 1.
The flag is hidden in window 0.
Use Ctrl-A 0 to switch to window 0!
MSG
hacker@terminal-multiplexing~switching-windows:~$  cat <<MSG
> Welcome to the window switching challenge!
> You are currently in window 1.
> The flag is hidden in window 0.
> Use Ctrl-A 0 to switch to window 0!
> MSG
Welcome to the window switching challenge!
You are currently in window 1.
The flag is hidden in window 0.
Use Ctrl-A 0 to switch to window 0!
hacker@terminal-multiplexing~switching-windows:~$  cat <<MSG
> Excellent work! You found window 0!
> Here is your flag: pwn.college{gwiu37954a2l_CvRphWGf6Lcy-7.0FO4IDOxwyNwEzNzEzW}
> MSG
Excellent work! You found window 0!
Here is your flag: pwn.college{gwiu37954a2l_CvRphWGf6Lcy-7.0FO4IDOxwyNwEzNzEzW}
[screen is terminating]
```

## Flag: 

```
pwn.college{gwiu37954a2l_CvRphWGf6Lcy-7.0FO4IDOxwyNwEzNzEzW}
```

### Notes:

`Key shortcuts:`
- `Ctrl-A c` — create a new window.
- `Ctrl-A n` — go to the next window.
- `Ctrl-A p` — go to the previous window.
- `Ctrl-A 0` … `Ctrl-A 9` — jump straight to a numbered window.
- `Ctrl-A "` — open a window selection menu.

# Challenge 5 Detaching and Attaching (tmux)

`tmux` is a modern terminal multiplexer similar to `screen` but it uses `Ctrl-B` as its command prefix; for this level you must launch a `tmux` session, detach it, run `/challenge/run` from your normal shell so the detached session receives the flag, then reattach to grab the prize.

## Solution:

- Launch a tmux session with `tmux`.
- Detach the session by holding `Ctrl`, pressing `B`, releasing both, then pressing `d`, shown as `Ctrl-B d`.
- In your normal terminal (outside tmux), run `/challenge/run`, this will secretly send the flag into the detached session.
- Reattach to the tmux session with `tmux attach` or `tmux a` to see the flag.
- If there are multiple tmux sessions, list them with `tmux ls` and attach to the correct one with `tmux attach -t <session>`.

#### Commands run: 

```sh
acker@terminal-multiplexing~detaching-and-attaching-tmux:~$ tmux
[detached (from session 0)]
hacker@terminal-multiplexing~detaching-and-attaching-tmux:~$ /challenge/run
Found detached tmux session: 0
Sending flag to your tmux session...

Flag sent! Now reattach to your tmux session with:
  tmux attach

You'll find the flag waiting for you there!
hacker@terminal-multiplexing~detaching-and-attaching-tmux:~$ tmux attach
hacker@terminal-multiplexing~detaching-and-attaching-tmux:~$ screen -r echo Congratulations, here is your flag: pwn.college{ctKE9RjA1srAecjJoY2N7gtgsfo.0VO4IDOxwyNwEzNzEzW}
[detached (from session 0)]
```

## Flag: 

```
pwn.college{ctKE9RjA1srAecjJoY2N7gtgsfo.0VO4IDOxwyNwEzNzEzW}
```

### Notes:

- do not run `/challenge/run` while attached to tmux, it must be run outside (in your normal shell) so the detached session receives the output.