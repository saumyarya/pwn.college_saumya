# Challenge 1 Launching Screen

`screen` is a terminal multiplexer that gives you virtual terminal windows inside your shell; for this level, simply launching a `screen` session is wired to present the flag automatically.

## Solution:

- Start a `screen` session with `screen`.
- Observe the output, the flag is printed automatically.
- When finished, leave the session with `exit` or `Ctrl-D`.

#### Commands run: 

```sh
Congratulations! You're inside a screen session!
Here's your flag:
pwn.college{MI8coYhrzVRIgTBeNES9OtYuZz5.0VN4IDOxwyNwEzNzEzW}
hacker@terminal-multiplexing~launching-screen:~$
```

## Flag: 

```
pwn.college{MI8coYhrzVRIgTBeNES9OtYuZz5.0VN4IDOxwyNwEzNzEzW}
```

### Notes:

- do not `sudo` or change file permissions, nothing extra is required.
- do not overcomplicate: this level just wants `screen` to be launched.

# Challenge 2 Detaching and Attaching

`screen` allows you to keep terminal sessions running even if you close or lose your connection. For this challenge, you’ll launch a session, detach it, run a command that secretly sends the flag there, and reattach to grab your prize. Detaching is done by pressing `Ctrl-A` then `d` (don’t panic, you’re just putting the session in the background). Reattaching is done with `screen -r`.

## Solution:

- Launch a screen session with `screen`.
- Do whatever the challenge tells you (here, just start it — no extra work).
- Detach the session with `Ctrl-A` then `d`. You should see `[detached from ...]`.
- While the session runs in the background, execute `/challenge/run` in your normal terminal, it secretly sends the flag to the detached session.

#### Commands run: 

```sh
hacker@terminal-multiplexing~detaching-and-attaching:~$ screen
[detached from 147.pts-0.terminal-multiplexing~detaching-and-attaching]
hacker@terminal-multiplexing~detaching-and-attaching:~$ /challenge/run
Found detached screen session: 147.pts-0.terminal-multiplexing~detaching-and-attaching
Sending flag to your screen session...

Flag sent! Now reattach to your screen session with:

  screen -r

You'll find the flag waiting for you there!
hacker@terminal-multiplexing~detaching-and-attaching:~$ screen -r
hacker@terminal-multiplexing~detaching-and-attaching:~$ echo Yes! Flag is: pwn.college{QRMELZMZeOm6lXwoB1N1TAEr6ZW.0lN4IDOxwyNwEzNzEzW}
Yes! Flag is: pwn.college{QRMELZMZeOm6lXwoB1N1TAEr6ZW.0lN4IDOxwyNwEzNzEzW}
[screen is terminating]
```

## Flag: 

```
pwn.college{QRMELZMZeOm6lXwoB1N1TAEr6ZW.0lN4IDOxwyNwEzNzEzW}
```

### Notes:

- `Ctrl-A` is the activation key for all screen shortcuts. If you see `[detached from ...]`, you did it right.

# Challenge 3 Finding Sessions

`screen` can run multiple sessions at once. This challenge gives you three sessions (two decoys, one contains the flag). You must list sessions, attach each candidate, detach after checking, and stop when you find the flag.

## Solution:

- Run `screen -ls` to list all sessions.
- Pick the first session id or name from the list and run `screen -r <id-or-name>` (for example `screen -r 23847.mysession` or `screen -r mysession`).
- Inspect the session for the flag (it should show up or you can run cat /flag if allowed).
- Detach with `Ctrl-A d` to leave it running in the background.
- Repeat for the next session until you find the flag.

#### Commands run: 

```sh
hacker@terminal-multiplexing~finding-sessions:~$ screen -ls
There are screens on:
        169.pts-0.terminal-multiplexing~launching-screen        (Remote or dead)
        144.session_b177b3acd1ed00d7    (Detached)
        147.session_8b9f5ebbffc3246a    (Detached)
        150.session_c9d060b6baeff106    (Detached)
4 Sockets in /home/hacker/.screen.
hacker@terminal-multiplexing~finding-sessions:~$ screen -r 144
[screen is terminating]
hacker@terminal-multiplexing~finding-sessions:~$ screen -r 147
hacker@terminal-multiplexing~finding-sessions:~$  echo 'Congratulations! You found the right session!'
Congratulations! You found the right session!
hacker@terminal-multiplexing~finding-sessions:~$  echo pwn.college{g6POHqsgTNqgQ9SUNgfILbkGbBG.01N4IDOxwyNwEzNzEzW}
pwn.college{g6POHqsgTNqgQ9SUNgfILbkGbBG.01N4IDOxwyNwEzNzEzW}
[screen is terminating]
```

## Flag: 

```
pwn.college{g6POHqsgTNqgQ9SUNgfILbkGbBG.01N4IDOxwyNwEzNzEzW}
```

### Notes:

- if `screen -r` says there are multiple matching sessions, give the full identifier (`PID.name`) to be explicit.

# Challenge 4 Switching Windows

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

# Challenge 6 Switching Windows (tmux)

`tmux` supports multiple windows per session (like tabs). For this challenge, a session has two windows: window 0 holds the flag, window 1 has a welcome message. You’ll attach, switch windows, and find the flag.

## Solution:

- Attach with `tmux attach` or `tmux a`.
- Switch to window 0 using `Ctrl-B 0` (or `Ctrl-B w` and select window 0).
- Inspect for the flag — it may already appear, or use `cat /flag` if allowed.
- Detach when finished with `Ctrl-B d` or exit the session cleanly with `exit`

#### Commands run: 

```sh
hacker@terminal-multiplexing~switching-windows-tmux:~$ tmux attach
 cat <<MSG
Welcome to the tmux window switching challenge!
You are currently in window 1.
The flag is hidden in window 0.
Use Ctrl-B 0 to switch to window 0!
MSG
hacker@terminal-multiplexing~switching-windows-tmux:~$  cat <<MSG
> Welcome to the tmux window switching challenge!
> You are currently in window 1.
> The flag is hidden in window 0.
> Use Ctrl-B 0 to switch to window 0!
> MSG
Welcome to the tmux window switching challenge!
You are currently in window 1.
The flag is hidden in window 0.
Use Ctrl-B 0 to switch to window 0!
hacker@terminal-multiplexing~switching-windows-tmux:~$  cat <<MSG
> Excellent work! You found window 0!
> Here is your flag: pwn.college{kqP7rdAK6ffra_HminGOMgo5zAu.0FM5IDOxwyNwEzNzEzW}
> MSG
Excellent work! You found window 0!
Here is your flag: pwn.college{kqP7rdAK6ffra_HminGOMgo5zAu.0FM5IDOxwyNwEzNzEzW}
[detached (from session challenge_session)]
```

## Flag: 

```
pwn.college{kqP7rdAK6ffra_HminGOMgo5zAu.0FM5IDOxwyNwEzNzEzW}
```

### Notes:

`Key shortcuts:`
- `Ctrl-B c` — create a new window.
- `Ctrl-B n` — go to the next window.
- `Ctrl-B p` — go to the previous window.
- `Ctrl-B 0` … `Ctrl-B 9` — jump directly to a numbered window.
- `Ctrl-B w` — show a nice window selection menu.