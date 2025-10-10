# Challenge 1 Listing Processes

`/challenge/run` was renamed to a random filename and you can't `ls /challenge`. But the process was launched, find its full path from the process list and execute it directly to get the flag. Use `ps` with widened output (the `w` option twice) so the command column isn't truncated.

## Solution:

- Show all processes with full, non-truncated command lines using `ps -efww` (or `ps auxww`).
- Filter for the `/challenge/` substring to find the running program's full path.
- Extract the `/challenge/<filename>` path from the matching line(s).
- Execute that path to relaunch the program and reveal the flag.

#### Commands run: 

```sh
hacker@processes~listing-processes:~$ ps aux
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.3  0.0   1056   640 ?        Ss   17:24   0:00 /sbin/docker-init -- /nix/var/nix/profiles/dojo-workspace/bin/dojo-init /run/do
root           7  0.1  0.0 231708  2560 ?        S    17:24   0:00 /run/dojo/bin/sleep 6h
root         132  0.0  0.0   4132  2560 ?        S    17:24   0:00 /challenge/3874-run-13876
root         135  0.0  0.0   2744  1600 ?        S    17:24   0:00 sleep 6h
hacker       146  0.1  0.0  36972 21760 ?        Sl   17:24   0:00 /nix/store/g0q8n7xfjp7znj41hcgrq893a9m0i474-ttyd-1.7.7/bin/ttyd --port 7681 --i
hacker       150  0.0  0.0 231940  4160 pts/0    Ss   17:24   0:00 /run/dojo/bin/bash --login
hacker       160  0.0  0.0 233600  3840 pts/0    R+   17:24   0:00 ps aux
hacker@processes~listing-processes:~$ /challenge/3874-run-13876
Yahaha, you found me! Here is your flag:
pwn.college{YzIJDIvD6f3u7P0ibs-RlR9Qoln.QX4MDO0wyNwEzNzEzW}
Now I will sleep for a while (so that you could find me with 'ps').
```

## Flag: 

```
pwn.college{YzIJDIvD6f3u7P0ibs-RlR9Qoln.QX4MDO0wyNwEzNzEzW}
```

### Notes:

- Use `-ww` (or `auxww`) to avoid `ps` truncating the CMD field; a single `w` may still truncate.

# Challenge 2 Killing Processes

`/challenge/run` refuses to execute while `/challenge/dont_run` is alive. Find the running `dont_run` process and terminate it so you can run the real binary and get the flag.

## Solution:

- List processes with full, non‑truncated command lines so you can see the exact `/challenge/dont_run` path (`ps -efww` or `ps auxww`).
- Extract the PID(s) of the process that mention `/challenge/dont_run`.
- Use `kill <PID>` to request graceful termination.
- Re-check the process list to confirm it's gone, then run `/challenge/run` to get the flag.

#### Commands run: 

```sh
hacker@processes~killing-processes:~$ ps auxww
USER         PID %CPU %MEM    VSZ   RSS TTY      STAT START   TIME COMMAND
root           1  0.0  0.0   1056   640 ?        Ss   17:32   0:00 /sbin/docker-init -- /nix/var/nix/profiles/dojo-workspace/bin/dojo-init /run/dojo/bin/sleep 6h
root           7  0.0  0.0 231708  2560 ?        S    17:32   0:00 /run/dojo/bin/sleep 6h
root         135  0.0  0.0   5204  3520 ?        S    17:32   0:00 su -c /challenge/.launcher hacker
hacker       136  0.0  0.0 231576  3520 ?        Ss   17:32   0:00 /challenge/dont_run
hacker       137  0.0  0.0 231708  2560 ?        S    17:32   0:00 sleep 6h
hacker       148  0.0  0.0  36972 21760 ?        Sl   17:32   0:00 /nix/store/g0q8n7xfjp7znj41hcgrq893a9m0i474-ttyd-1.7.7/bin/ttyd --port 7681 --interface 0.0.0.0 --writable -t disableLeaveAlert true /run/dojo/bin/bash --login
hacker       152  0.1  0.0 232164  4160 pts/0    Ss   17:32   0:00 /run/dojo/bin/bash --login
hacker       162  0.0  0.0 233600  3840 pts/0    R+   17:36   0:00 ps auxww
hacker@processes~killing-processes:~$ kill 136
hacker@processes~killing-processes:~$ /challenge/run
Great job! Here is your payment:
pwn.college{QWqQ0VPGA9MoKPlCFusSft7Ltdv.QXyQDO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{QWqQ0VPGA9MoKPlCFusSft7Ltdv.QXyQDO0wyNwEzNzEzW}
```

### Notes:

- use `ps -efww` or `ps auxww` to avoid truncation of the CMD/COMMAND column so you can see the full `/challenge/dont_run` path.

# Challenge 3 Interrupting Processes

Some programs block the terminal and won’t stop on their own. Instead of killing them with `kill`, you can use a shortcut to make them exit immediately. In Linux, pressing `Ctrl-C` sends an interrupt signal to the program in the terminal, making it stop.
Try running `/challenge/run` and use `Ctrl-C` to stop it, the flag will appear after the interrupt.

## Solution:

- Started the challenge with `/challenge/run`.
- The program kept running without giving the flag.
- Remembered the hint about `Ctrl-C` for interrupting terminal processes.
- Pressed Ctrl-C to stop the running program.
- The program exited and displayed the flag.

#### Commands run: 

```sh
hacker@processes~interrupting-processes:~$ /challenge/run
I could give you the flag... but I won't, until this process exits. Remember, 
you can force me to exit with Ctrl-C. Try it now!
^C
Good job! You have used Ctrl-C to interrupt this process! Here is your flag:
pwn.college{cQwaDFd7Wk7q-Z8Pqh9ouMNAp2l.QXzQDO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{cQwaDFd7Wk7q-Z8Pqh9ouMNAp2l.QXzQDO0wyNwEzNzEzW}
```

### Notes:

- `Ctrl-C` interrupts the program currently running in the terminal.
- Doesn’t require the process ID like kill does.
- Works for programs waiting for input or stuck in loops.

# Challenge 4 Killing Misbehaving Processes

A decoy process `/challenge/decoy` is holding the named pipe `/tmp/flag_fifo`, preventing `/challenge/run` from writing the real flag to it. Kill the decoy process, then run `/challenge/run` so it can write the flag into `/tmp/flag_fifo` for you to read.

## Solution:

- We need the PID of the misbehaving process `/challenge/decoy` and to kill it.
- Use process listing / pattern matching (`ps`/`pgrep`) to find the PID reliably.
- Kill the PID gracefully first (`kill PID`). If it persists, escalate (`kill -9 PID`).
- The FIFO is buffered, there may be decoy data already in the pipe after killing the process. Wait ~1s if you still see decoys.
- Run `/challenge/run` (it writes to `/tmp/flag_fifo` itself). Read the FIFO (`cat /tmp/flag_fifo`) to see the flag.

#### Commands run: 

```sh
hacker@processes~killing-misbehaving-processes:~$ ps -ef | grep /challenge/decoy
root         139       1  0 16:55 ?        00:00:00 su -c exec /challenge/decoy > /tmp/flag_fifo hacker
hacker       142     139  0 16:55 ?        00:00:00 /usr/bin/python /challenge/decoy
hacker       169     157  0 16:56 pts/0    00:00:00 grep --color=auto /challenge/decoy
hacker@processes~killing-misbehaving-processes:~$ kill 142
hacker@processes~killing-misbehaving-processes:~$ /challenge/run
Sending the flag to /tmp/flag_fifo!
hacker@processes~killing-misbehaving-processes:~$ cat /tmp/flag_fifo
pwn.college{UMa5zqopEYwymCjWgjstpK_YGwG.0FNzMDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{UMa5zqopEYwymCjWgjstpK_YGwG.0FNzMDOxwyNwEzNzEzW}
```

### Notes:

- Pipes are buffered: killing the decoy may still let some decoy output flow through for a second. Wait ~1s and re-run /challenge/run if needed.

# Challenge 5 Suspending Processes

The level expects two copies of `/challenge/run` to be launched from the same terminal at the same time. Do this by starting one, suspending it with Ctrl-Z (so it’s stopped but still “owned” by your terminal), then launching a second copy in the same terminal.

## Solution:

- Start `/challenge/run` in the foreground.
- Suspend it with Ctrl-Z (sends SIGTSTP, the process becomes Stopped).
- Launch another `/challenge/run` from the same terminal. now two copies have been started from the same terminal (one stopped, one running).

#### Commands run: 

```sh
hacker@processes~suspending-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running in 
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root         146     136  0 17:16 pts/0    00:00:00 bash /challenge/run
root         148     146  0 17:16 pts/0    00:00:00 ps -f

I don't see a second me!

To pass this level, you need to suspend me and launch me again! You can 
background me with Ctrl-Z or, if you're not ready to do that for whatever 
reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~suspending-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running in 
this terminal... Let's check!

UID          PID    PPID  C STIME TTY          TIME CMD
root         146     136  0 17:16 pts/0    00:00:00 bash /challenge/run
root         153     136  0 17:16 pts/0    00:00:00 bash /challenge/run
root         155     153  0 17:16 pts/0    00:00:00 ps -f

Yay, I found another version of me! Here is the flag:
```

## Flag: 

```
pwn.college{M9_IsiY4ILdcUJjMCxMbk7A-1qd.QX1QDO0wyNwEzNzEzW}
```

### Notes:

- Ctrl-Z suspends (stops) the process but does not kill it. it remains associated with your terminal.


# Challenge 6 Resuming Processes

This level teaches how to resume a process after suspending it.
You’ll start /challenge/run, suspend it with Ctrl-Z, and then bring it back to the foreground with the fg command.

## Solution:

- Launch `/challenge/run` normally in the terminal, it will start running.
- Press Ctrl-Z to suspend it (this sends SIGTSTP and stops the process temporarily).
- Resume it in the foreground using `fg` this continues the process in the same terminal.
- The challenge will detect that the process was resumed and give the flag.

#### Commands run: 

```sh
hacker@processes~resuming-processes:~$ /challenge/run
Let's practice resuming processes! Suspend me with Ctrl-Z, then resume me with 
the 'fg' command! Or just press Enter to quit me!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~resuming-processes:~$ fg
/challenge/run
I'm back! Here's your flag:
pwn.college{YwNPNO1ZM9EDlkzBx0ApkiDJ-Ve.QX2QDO0wyNwEzNzEzW}
Don't forget to press Enter to quit me!

Goodbye!
```

## Flag: 

```
pwn.college{YwNPNO1ZM9EDlkzBx0ApkiDJ-Ve.QX2QDO0wyNwEzNzEzW}
```

### Notes:

- Ctrl-Z suspends (does not kill) the process, it remains in the background as “Stopped.”
- fg resumes the most recent stopped job by default, or you can specify a job number: fg %1.

# Challenge 7 Backgrounding Processes

You’ve already learned to suspend (Ctrl-Z) and resume (fg) a process.
Now, you’ll resume it in the background using bg, so it keeps running while you get your shell prompt back.

The challenge expects:
- One /challenge/run running in the background, not suspended.
- Then another /challenge/run launched in the same terminal.

## Solution:

- Start `/challenge/run` in the terminal, it’ll start running.
- Suspend it using Ctrl-Z (it becomes stopped).
- Resume it in the background using `bg` (it continues running without blocking your shell).
- While it’s running in the background, launch another `/challenge/run` in the same terminal.
- The challenge will detect both; one backgrounded, one foreground; and give the flag.

#### Commands run: 

```sh
hacker@processes~backgrounding-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and 
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root         146 S+   bash /challenge/run
root         148 R+   ps -o user=UID,pid,stat,cmd

I don't see a second me!

To pass this level, you need to suspend me, resume the suspended process in the 
background, and then launch a new version of me! You can background me with 
Ctrl-Z (and resume me in the background with 'bg') or, if you're not ready to 
do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~backgrounding-processes:~$ bg
[1]+ /challenge/run &
hacker@processes~backgrounding-processes:~$ 


Yay, I'm now running the background! Because of that, this text will probably 
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times 
to scroll this text out.

hacker@processes~backgrounding-processes:~$ /challenge/run
I'll only give you the flag if there's already another copy of me running *and 
not suspended* in this terminal... Let's check!

UID          PID STAT CMD
root         146 S    bash /challenge/run
root         156 S    sleep 6h
root         157 S+   bash /challenge/run
root         159 R+   ps -o user=UID,pid,stat,cmd

Yay, I found another version of me running in the background! Here is the flag:
```

## Flag: 

```
pwn.college{EOzDKPCvi6whu4_nfwk18H6PQjF.QX3QDO0wyNwEzNzEzW}
```

### Notes:

- Ctrl-Z suspends the process (T state in ps).
- `bg` resumes it without bringing it to foreground, letting your shell stay free.
- You can verify state with ps -o stat,cmd:
    - T - stopped (suspended)
    - S - sleeping (running normally)
    - R - actively running
    - + - foreground process

# Challenge 8 Foregrounding Processes

You’ve learned to suspend processes (Ctrl-Z), resume them in the background (bg), and now — you’ll bring a backgrounded process back to the foreground using fg.

This challenge’s /challenge/run wants you to:
- Run it.
- Background it with bg.
- Bring it back to the foreground with fg.

## Solution:

- Start `/challenge/run` in your shell.
- Suspend it with Ctrl-Z (it stops temporarily).
- Resume it in the background using `bg`.
- Bring it back to the foreground with `fg`.
- The challenge will detect that the process was backgrounded and then foregrounded correctly, and give you the flag

#### Commands run: 

```sh
hacker@processes~foregrounding-processes:~$ /challenge/run
To pass this level, you need to suspend me, resume the suspended process in the 
background, and *then* foreground it without re-suspending it! You can 
background me with Ctrl-Z (and resume me in the background with 'bg') or, if 
you're not ready to do that for whatever reason, just hit Enter and I'll exit!
^Z
[1]+  Stopped                 /challenge/run
hacker@processes~foregrounding-processes:~$ bg
[1]+ /challenge/run &
hacker@processes~foregrounding-processes:~$ 


Yay, I'm now running the background! Because of that, this text will probably 
overlap weirdly with the shell prompt. Don't panic; just hit Enter a few times 
to scroll this text out. After that, resume me into the foreground with 'fg'; 
I'll wait.

hacker@processes~foregrounding-processes:~$ fg
/challenge/run
YES! Great job! I'm now running in the foreground. Hit Enter for your flag!

pwn.college{4-iskGPvLlEWdwSk8fbyiuG88Nf.QX4QDO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{4-iskGPvLlEWdwSk8fbyiuG88Nf.QX4QDO0wyNwEzNzEzW}
```

### Notes:

- Ctrl-Z - suspend (process stops).
- `bg` - background (process resumes running, you get shell back).
- `fg` - foreground (process resumes in front; you interact with it again).
- `jobs` - lists all background and suspended jobs.

# Challenge 9 Starting Backgrounded Processes

You’ve learned to suspend and resume processes,now you’ll start one already in the background.
Appending an ampersand (`&`) to a command tells the shell to run it without blocking the terminal.

Challenge's requirement:
- Launch /challenge/run in the background.
- Once it’s running, it’ll print your flag automatically.

## Solution:

- Start `/challenge/run` normally, but add `&` at the end to make it backgrounded immediately.
- The shell will print a job number like [1] 149, that means it’s running in the background.
- You can verify that it’s running with `jobs` or `ps -o stat,cmd`.
- The process will run and output your flag.

#### Commands run: 

```sh
hacker@processes~starting-backgrounded-processes:~$ /challenge/run
You've started me in the foreground! You must start me in the background (by 
appending '&' to the command) to get the flag!
hacker@processes~starting-backgrounded-processes:~$ /challenge/run &
[1] 149
hacker@processes~starting-backgrounded-processes:~$ 


Yay, you started me in the background! Because of that, this text will probably 
overlap weirdly with the shell prompt, but you're used to that by now...

Anyways! Here is your flag!
pwn.college{wmT7NV0n4GCDYoNX3VCSVYvGvEo.QX5QDO0wyNwEzNzEzW}

[1]+  Done                    /challenge/run
```

## Flag: 

```
pwn.college{wmT7NV0n4GCDYoNX3VCSVYvGvEo.QX5QDO0wyNwEzNzEzW}
```

### Notes:

- `&` runs the command directly in the background, no need to Ctrl-Z or `bg`.

# Challenge 10 Process Exit Codes

Every shell command exits with a status code:
- 0 - success
- non-zero - failure or a specific error
You can read the exit code of the most recent command using the special variable `$?`.

Your task:
- Run `/challenge/get-code`
- Check its exit code using `echo $?`
- Pass that code as an argument to `/challenge/submit-code`

## Solution:

- Run `/challenge/get-code`, it will execute silently but set an exit code internally.
- Immediately after it finishes, use `echo $?` to print the exit code.
- Take that number and use it as an argument to `/challenge/submit-code`.
- Example: if the code was 27, you would run `/challenge/submit-code 27`.

#### Commands run: 

```sh
acker@processes~process-exit-codes:~$ /challenge/get-code
Exiting with an error code!
hacker@processes~process-exit-codes:~$ echo $?
2
hacker@processes~process-exit-codes:~$ /challenge/submit-code 2
CORRECT! Here is your flag:
pwn.college{EElLIjpuoxuOqQpSLU11XE8Dkan.QX5YDO1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{EElLIjpuoxuOqQpSLU11XE8Dkan.QX5YDO1wyNwEzNzEzW}
```

### Notes:

- `$?` always stores the exit code of the last command, run `echo $?` immediately after `/challenge/get-code`.
