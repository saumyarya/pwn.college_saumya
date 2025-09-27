# Challenge 1 The Root

In this level we learn about **absolute paths**. The filesystem root is `/`. A program placed directly under `/` can be invoked by giving its absolute path `/pwn`. The goal of this challenge is to run `/pwn` from the remote shell and capture the flag it prints.

```sh
hacker@paths~the-root:~$ /pwn
BOOM!!!
Here is your flag:
```

## Solution:

- The challenge hints that a program named `pwn` has been placed in the filesystem root `/`.
- To run a program anywhere on the system you can either rely on `PATH` (for commands) or provide an **absolute path** starting with `/`.
- Since the program is at `/pwn`, invoking `/pwn` directly will execute it and print the flag.

## Flag: 

```
pwn.college{MKK0VxeyVzFSDSu40rsBmjppXHZ.QX4cTO0wyNwEzNzEzW}
```

### Notes:

-I learnt about invoking the absolute path.

# Challenge 2 Program and absolute paths

In this level the challenge program lives inside a directory placed at the filesystem root: `/challenge/run`. The goal is to execute the program using its **absolute path** and capture the flag it prints.

```sh
hacker@paths~program-and-absolute-paths:~$ /challenge/run
Correct!!!
/challenge/run is an absolute path! Here is your flag:
```

## Solution:

- The challenge directory is located at `/challenge`, and the program name is `run`.
- To execute it regardless of the shell `PATH`, call it by its **absolute path** `/challenge/run`.
- Running `/challenge/run` will execute the program and print the flag.

## Flag: 

```
pwn.college{0vMIxyj_6Hxehqg2MU7Ap2ga2pH.QX1QTN0wyNwEzNzEzW}
```

### Notes:

-I learnt how to execute the challenge by executing it's absolute path.

# Challenge 3 Position thy self

This level teaches how the shell's **current working directory** affects program execution. The challenge requires changing the shell's working directory to a specific path (the challenge tells you which one), and then running the challenge program. Absolute paths ignore the current directory, but some programs expect to be run from a particular working directory.

```sh
hacker@paths~position-thy-self:~$ cd /usr/share/zoneinfo/posix/Asia
hacker@paths~position-thy-self:/usr/share/zoneinfo/posix/Asia$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
```

## Solution:

- Read the challenge prompt to learn the required working directory (replace `REPLACE_WITH_DIR` below with the actual path the challenge requires).
- Use `cd` to change the shell's current working directory to that path.
- Verify the current directory with `pwd` and optionally list files with `ls`.
- Execute the challenge program `/challenge/run` while the shell's current working directory is the required directory.
- Copy the flag output exactly.

## Flag: 

```
pwn.college{A7GJK9vxL6R8dkTHIr57Fsjf9OT.QX2QTN0wyNwEzNzEzW}
```

### Notes:

-Some programs expect to be launched from a specific working directory
-Mistake to avoid: running the program from the wrong directory — it may fail or print a different output.
-If unsure what directory the challenge expects, re-check the challenge prompt, it usually states the required path.

# Challenge 4 Position elsewhere

This level teaches how the shell's **current working directory** affects program execution. The challenge requires changing the shell's working directory to a specific path (the challenge tells you which one), and then running the challenge program. Absolute paths ignore the current directory, but some programs expect to be run from a particular working directory.

```sh
hacker@paths~position-elsewhere:~$ /challenge/run
Incorrect...
You are not currently in the /tmp directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-elsewhere:~$ cd /tmp
hacker@paths~position-elsewhere:/tmp$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
```

## Solution:

- Read the challenge prompt to learn the required working directory (replace `REPLACE_WITH_DIR` below with the actual path the challenge requires).
- Use `cd` to change the shell's current working directory to that path.
- Verify the current directory with `pwd` and optionally list files with `ls`.
- Execute the challenge program `/challenge/run` while the shell's current working directory is the required directory.
- Copy the flag output exactly.

## Flag: 

```
pwn.college{QY2G_PwjmV0NOna1jxGTVQ3Xsa3.QX3QTN0wyNwEzNzEzW}
```

### Notes:

-Some programs expect to be launched from a specific working directory
-Mistake to avoid: running the program from the wrong directory — it may fail or print a different output.
-If unsure what directory the challenge expects, re-check the challenge prompt, it usually states the required path.

# Challenge 5 Position yet elsewhere

This level teaches how the shell's **current working directory** affects program execution. The challenge requires changing the shell's working directory to a specific path (the challenge tells you which one), and then running the challenge program. Absolute paths ignore the current directory, but some programs expect to be run from a particular working directory.

```sh
hacker@paths~position-yet-elsewhere:~$ /challenge/run
Incorrect...
You are not currently in the /proc/138/fd directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~position-yet-elsewhere:~$ cd /proc/138/fd
hacker@paths~position-yet-elsewhere:/proc/138/fd$ /challenge/run
Correct!!!
/challenge/run is an absolute path, invoked from the right directory!
Here is your flag:
```

## Solution:

- Read the challenge prompt to learn the required working directory (replace `REPLACE_WITH_DIR` below with the actual path the challenge requires).
- Use `cd` to change the shell's current working directory to that path.
- Verify the current directory with `pwd` and optionally list files with `ls`.
- Execute the challenge program `/challenge/run` while the shell's current working directory is the required directory.
- Copy the flag output exactly.

## Flag: 

```
pwn.college{s7FUPK17cvyje5DI_lBpSyxtfbg.QX4QTN0wyNwEzNzEzW}
```

### Notes:

-Some programs expect to be launched from a specific working directory
-Mistake to avoid: running the program from the wrong directory — it may fail or print a different output.
-If unsure what directory the challenge expects, re-check the challenge prompt, it usually states the required path.

# Challenge 6 Implicit relative paths, from /

This level emphasizes **relative paths**. A relative path does not start with `/` and is interpreted relative to your current working directory (cwd). For this challenge you must set your cwd to `/` and invoke the challenge program using a relative path that starts with `c` (i.e., `challenge/run`).

```sh
hacker@paths~implicit-relative-paths-from-:~$ /challenge/run
Incorrect...
You are not currently in the / directory.
Please use the `cd` utility to change directory appropriately.
hacker@paths~implicit-relative-paths-from-:~$ cd /
hacker@paths~implicit-relative-paths-from-:/$ challenge/run
Correct!!!
challenge/run is a relative path, invoked from the right directory!
Here is your flag:
```

## Solution:

- The prompt tells us to run the program using a **relative** path while our current working directory is `/`.
- A relative path is resolved from the current directory; when cwd is `/`, the relative path `challenge/run` refers to `/challenge/run`.
- Therefore, change to `/` with `cd /`, confirm with `pwd`, and then run the program using the relative path `challenge/run` (no leading slash).
- Copy the exact flag output and record it in this report.

## Flag: 

```
pwn.college{saDkEJvlGyMfh9OC_APEDiv9Z7q.QX5QTN0wyNwEzNzEzW}
```

### Notes:

- A relative path is interpreted from the current working directory; challenge/run from ```/``` equals ```/challenge/run```.
- ```cd /``` sets the cwd to root; confirm with ```pwd```
- Mistake to avoid: using ```/challenge/run``` (absolute) when the level explicitly requires a relative invocation.

# Challenge 7 Explicit relative paths, from /

This level demonstrates how `.` (dot) refers to the **current directory** and how it can be used inside relative paths. A path that includes `.` still resolves relative to your current working directory; `./challenge/run` and `challenge/run` are equivalent when your cwd is `/`.

```sh
hacker@paths~explicit-relative-paths-from-:~$ cd /
hacker@paths~explicit-relative-paths-from-:/$ ./challenge
bash: ./challenge: Is a directory
hacker@paths~explicit-relative-paths-from-:/$ ./challenge/run
Correct!!!
./challenge/run is a relative path, invoked from the right directory!
Here is your flag:
```

## Solution:

- `.` represents the current directory. It allows you to be explicit that the path is relative to the cwd.
- When your current working directory is `/`, relative paths like `challenge/run`, `./challenge/run`, or `././challenge/run` all resolve to `/challenge/run`.
- To meet the challenge requirement, change to the correct working directory if needed and invoke the program using a relative path that includes `.`.

## Flag: 

```
pwn.college{AaRtj4Kph9fPwgyKFBk9ElqhI-4.QXwUTN0wyNwEzNzEzW}
```

### Notes:

- ```.``` literally means “current directory,” so ```./challenge/run``` is exactly the same as ```challenge/run``` if you’re in ```/```.
- It’s just more explicit and safer.

# Challenge 8 Implicit relative path

This level demonstrates why Linux doesn't automatically search the current directory for executables when you type a "naked" command. To safely run a program that is in the current directory, you must explicitly reference it using `.` (dot), e.g. `./program`. The challenge requires changing to `/challenge` and executing the `run` program explicitly from that directory.

```sh
hacker@paths~implicit-relative-path:~$ cd /challenge
hacker@paths~implicit-relative-path:/challenge$ run
bash: run: command not found
hacker@paths~implicit-relative-path:/challenge$ ./run
Correct!!!
./run is a relative path, invoked from the right directory!
Here is your flag:
```

## Solution:

- Linux intentionally does **not** search the current directory for commands by default to avoid accidentally executing malicious or name-conflicting programs.
- When in `/challenge`, typing `run` will **not** execute `/challenge/run` because the shell only searches locations in `$PATH`.
- To run the local file in the current directory, explicitly invoke it with `./run`.
- Steps: `cd /challenge`, confirm cwd with `pwd`, execute `./run`, copy the printed flag.

## Flag: 

```
pwn.college{QrudKypButUwcLGaUaMRIoX9GCd.QXxUTN0wyNwEzNzEzW}
```

### Notes:

- Linux does not search the current directory for executables by default to avoid accidental execution of local files that shadow system commands.
- Use ```./program``` to explicitly run a program in the current directory.
- Mistake to avoid: assuming ```run``` will execute without ```./``` while inside ```/challenge```.

# Challenge 9 Home sweet home

This level tests `~` expansion and constraints on argument length. The program `/challenge/run` will write the flag to a file whose path you supply as an argument. Constraints:

- After expansion the path must be an absolute path inside your home directory.
- Before expansion your argument must be three characters or less.

```sh
hacker@paths~home-sweet-home:~$ /challenge/run
You must provide an argument to /challenge/run when you invoke it!
hacker@paths~home-sweet-home:~$ /challenge/run ~/t
Writing the file to /home/hacker/t!
... and reading it back to you:
```

## Solution:

- The program writes the flag to a file at the path you give it.
- `~` expands to your home directory (`/home/hacker`) when it appears at the start of an argument.
- The challenge requires the pre-expansion argument to be ≤3 characters. `~/t` is exactly three characters and expands to `/home/hacker/t`.
- Therefore, call `/challenge/run` with `~/t` as the argument. Then view the file `/home/hacker/t` (or `~/t`) to read the flag.

## Flag: 

```
pwn.college{M2UY6Du6S_ssKmmLC6RAXxw1ViY.QXzMDO0wyNwEzNzEzW}
```

### Notes:

- Pre-expansion constraint (≤3 chars) is satisfied by ```~/t``` (three characters).
- After expansion the path becomes absolute and inside home (good for the “absolute path” requirement).