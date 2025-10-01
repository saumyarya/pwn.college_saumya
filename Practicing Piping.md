# Challenge 1 Redirecting output

Use shell output redirection (`>`) to write the exact word `PWN` (uppercase) into a file named `COLLEGE` (uppercase). After writing, read the file to confirm it contains PWN.

## Solution:

- The shell operator `>` takes the stdout of a command and writes it to a file, creating the file if it doesn't exist and overwriting it if it does.
- `echo` writes its arguments to stdout followed by a newline. `echo PWN > COLLEGE` will therefore create (or overwrite) a file called `COLLEGE` containing `PWN` and a trailing newline.

#### Commands run: 

```sh
hacker@piping~redirecting-output:~$ echo PWN > COLLEGE
Correct! You successfully redirected 'PWN' to the file 'COLLEGE'! Here is your 
flag:
```

## Flag: 

```
pwn.college{s_gbuu_wuocEHeFZIc8hcfw3Eva.QX0YTN0wyNwEzNzEzW}
```

### Notes:

- `>` overwrites the target file. Use `>>` to append instead.
- `echo` adds a newline at the end; `printf PWN` does not. Choose based on whether you want the newline.

# Challenge 2 Redirecting more output

`/challenge/run` prints the flag on stdout and prints its instructions/feedback on stderr. Your job: run `/challenge/run` so the flag (stdout) is written into a file named `myflag`. After that, `cat myflag` to get the flag.

## Solution:

- In Unix, `>` redirects stdout into a file. Stderr is unaffected.
- Since the flag is printed to stdout,` >/path/to/myflag` will capture only the flag into the file while the program’s messages (stderr) still appear on the terminal.

#### Commands run: 

```sh
hacker@piping~redirecting-more-output:~$ /challenge/run > myflag
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : myflag
[INFO] - the challenge will output a reward file if all the tests pass : /flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /flag file.

[TEST] You should have redirected my stdout to a file called myflag. Checking...

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
hacker@piping~redirecting-more-output:~$ cat myflag

[FLAG] Here is your flag:
```

## Flag: 

```
pwn.college{0fjwk5eDPRKgxG2ljQHY2cgKdPi.QX1YTN0wyNwEzNzEzW}
```

### Notes:

- `>` overwrites myflag (creates it if missing).
- `2>/dev/null` discards stderr so your terminal stays quiet while the flag still goes into the file.
- `>myflag 2>&1` or `&>myflag` redirects both stdout and stderr into myflag, the flag will be there but mixed with other messages.

# Challenge 3 Appending output

`/challenge/run` writes the first half of the flag into `/home/hacker/the-flag` (internally) and prints the second half to stdout. If you redirect stdout with truncation (`>`), the second half will overwrite the first half in the file and you’ll lose the first half. Use append redirection (`>>`) so the second half is appended to the already-written first half and the full flag ends up in `/home/hacker/the-flag`.

## Solution:

- `>` truncates (creates/overwrites) a file before the command runs.
- `>>` opens the file for append, preserving existing contents and writing new stdout at the end.
- Because the program has already placed the first half into `/home/hacker/the-flag`, you must append the program’s stdout (the second half) to the same file otherwise truncation will destroy the first half.
- So the correct interactive command uses `>> /home/hacker/the-flag`. Optionally silence stderr (`2>/dev/null`) if you don’t want on-screen chatter.
- After running, inspect `/home/hacker/the-flag` to see the complete flag

#### Commands run: 

```sh
hacker@piping~appending-output:~$ /challenge/run >> ~/the-flag
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : /home/hacker/the-flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] Good luck!

[TEST] You should have redirected my stdout to a file called /home/hacker/the-flag. Checking...

[HINT] File descriptors are inherited from the parent, unless the FD_CLOEXEC is set by the parent on the file descriptor.
[HINT] For security reasons, some programs, such as python, do this by default in certain cases. Be careful if you are
[HINT] creating and trying to pass in FDs in python.

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
I will write the flag in two parts to the file /home/hacker/the-flag! I'll do 
the first write directly to the file, and the second write, I'll do to stdout 
(if it's pointing at the file). If you redirect the output in append mode, the 
second write will append to (rather than overwrite) the first write, and you'll 
get the whole flag! 

hacker@piping~appending-output:~$ cat ~/the-flag
 | 
\|/ This is the first half:
 v 
pwn.college{o0kBZgiEhEUaaCUARocmglKYiwo.QX3ATO0wyNwEzNzEzW}
                              ^
     that is the second half /|\
                              |

If you only see the second half above, you redirected in *truncate* mode (>) 
rather than *append* mode (>>), and so the write of the second half to stdout 
overwrote the initial write of the first half directly to the file. Try append 
mode!
```

## Flag: 

```
pwn.college{o0kBZgiEhEUaaCUARocmglKYiwo.QX3ATO0wyNwEzNzEzW}
```

### Notes:

- `>` truncates the target file before the command runs — dangerous if you need existing contents preserved.
- `>>` safely appends; use it when you want to preserve earlier output.
- If `cat /home/hacker/the-flag` shows only a partial flag, you likely used `>` (truncation), re-run the append command.

# Challenge 4 Redirecting errors

Run `/challenge/run` so that stdout (the flag) goes to the file `myflag` and stderr (the program’s instructions/feedback) goes to the file `instructions`. When done correctly nothing should print to your terminal, the flag will be in `myflag` and the instructions in `instructions`.

## Solution:

- FD 1 is stdout and FD 2 is stderr. Using `>` without a number is shorthand for `1>`.
- To send stdout to `myflag` and stderr to `instructions` you must explicitly redirect both file descriptors: `> myflag 2> instructions`.
- The shell performs redirections before running the program, so both files will be created (or truncated) and the program’s outputs will be written into them.
- Nothing will appear on your terminal because both standard channels have been redirected. Inspect `myflag` for the flag and `instructions` for the program’s messages.
- You can also write `1> myflag 2> instructions`, it’s equivalent and sometimes clearer.
- Don’t use a single redirect like `&> file` (or `>file 2>&1`) if you want the outputs separated — that would mix stdout and stderr together.

#### Commands run: 

```sh
hacker@piping~redirecting-errors:~$ /challenge/run > myflag 2> instructions
hacker@piping~redirecting-errors:~$ cat myflag

[FLAG] Here is your flag:
[FLAG] pwn.college{wVbuEPnOH7lEHevI3RfqodXxJVO.QX3YTN0wyNwEzNzEzW}

hacker@piping~redirecting-errors:~$ cat instructions
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : myflag
[INFO] - the challenge will check that error output is redirected to a specific file path : instructions
[INFO] - the challenge will output a reward file if all the tests pass : /flag

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /flag file.

[TEST] You should have redirected my stdout to a file called myflag. Checking...

[PASS] The file at the other end of my stdout looks okay!

[TEST] You should have redirected my stderr to instructions. Checking...

[PASS] The file at the other end of my stderr looks okay!
[PASS] Success! You have satisfied all execution requirements.
```

## Flag: 

```
pwn.college{wVbuEPnOH7lEHevI3RfqodXxJVO.QX3YTN0wyNwEzNzEzW}
```

### Notes:

- `>` = `1>` (stdout). `2>` redirects stderr. Order of `> file 2>` other does not matter.
- To combine stdout and stderr into one file use `>file 2>&1` (or `&>file` in some shells), not what you want here if you need them separated.

# Challenge 5 Redirecting input

Create a file named `PWN` whose contents are the exact word `COLLEGE`. Then run `/challenge/run` so that it reads its input from the `PWN` file using input redirection (`<`). The program will consume the contents of `PWN` and (if correct) print the flag.

## Solution:

- Input redirection with `<` makes the program read from a file as if that file were typed on stdin.
- First you must create `PWN` containing `COLLEGE`. The simplest ways are `echo` (adds a trailing newline) or `printf` (no trailing newline). Either is usually fine unless the challenge strictly requires no newline.
- After `PWN` exists, run `/challenge/run < PWN` so the program reads the word `COLLEGE` from stdin.
- If the program prints the flag to stdout, it will appear on your terminal; you can redirect stdout to a file if you want to save it (e.g., `> myflag`).
- Verify file contents with `cat PWN` before running to avoid mistakes.

#### Commands run: 

```sh
hacker@piping~redirecting-input:~$ echo COLLEGE > PWN
hacker@piping~redirecting-input:~$ /challenge/run < PWN
Reading from standard input...
Correct! You have redirected the PWN file into my standard input, and I read 
the value 'COLLEGE' out of it!
Here is your flag:
```

## Flag: 

```
pwn.college{ESi4bwoHlXF-2zoobejcfVq3OlR.QXwcTN0wyNwEzNzEzW}
```

### Notes:

- Verify `PWN` with `cat PWN`before running `/challenge/run`. Typos in the file will cause the challenge to fail.
- To both feed input and capture the flag, combine redirections: `/challenge/run < PWN > myflag`.

# Challenge 6 Grepping stored results

Run `/challenge/run` and redirect its (large) stdout to `/tmp/data.txt`. The file will contain 100,000 lines; one of those lines contains the flag. Use `grep` to find the flag inside `/tmp/data.txt`.

## Solution:

- Use `>` to redirect stdout into `/tmp/data.txt`. This will create/truncate `/tmp/data.txt` and stream the program’s stdout there.
- Because the program prints a lot of data, it’s efficient to let it write to a file and then search that file with `grep` rather than trying to watch the program live.
- Use a targeted `grep` pattern for the flag format (for example `pwn.college{...}`).

#### Commands run: 

```sh
hacker@piping~grepping-stored-results:~$ /challenge/run > /tmp/data.txt
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge will check that output is redirected to a specific file path : /tmp/data.txt
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stdout to a file called /tmp/data.txt. Checking...

[HINT] File descriptors are inherited from the parent, unless the FD_CLOEXEC is set by the parent on the file descriptor.
[HINT] For security reasons, some programs, such as python, do this by default in certain cases. Be careful if you are
[HINT] creating and trying to pass in FDs in python.

[PASS] The file at the other end of my stdout looks okay!
[PASS] Success! You have satisfied all execution requirements.
hacker@piping~grepping-stored-results:~$ grep pwn.college /tmp/data.txt
pwn.college{EFtzlTBvGulBcDo-KFmBkmTsHyr.QX4EDO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{EFtzlTBvGulBcDo-KFmBkmTsHyr.QX4EDO0wyNwEzNzEzW}
```

### Notes:

- Learnt redirecting output using `>` and grepping the flag among a 100,000 lines.

# Challenge 7 Grepping live output

Instead of writing `/challenge/run`’s huge output to a file and then searching it, connect its stdout directly into `grep` using a pipe (`|`). `grep` will read the stream and print the flag as soon as it sees it. The goal: run `/challenge/run` and pipe its output into a `grep` that finds the `pwn.college{...}` flag.

## Solution:

- The pipe `|` connects stdout (FD 1) of the left command to stdin (FD 0) of the right command. No intermediate file is needed.
- Because `/challenge/run` emits ~100k lines and one contains the flag, piping into `grep` lets you search the stream in real time.

#### Commands run: 

```sh
hacker@piping~grepping-live-output:~$ /challenge/run | grep pwn.college
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge checks for a specific process at the other end of stdout : grep
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stdout to another process. Checking...
[TEST] Performing checks on that process!

[INFO] The process' executable is /nix/store/8b4vn1iyn6kqiisjvlmv67d1c0p3j6wj-gnugrep-3.11/bin/grep.
[INFO] This might be different than expected because of symbolic links (for example, from /usr/bin/python to /usr/bin/python3 to /usr/bin/python3.8).
[INFO] To pass the checks, the executable must be grep.

[PASS] You have passed the checks on the process on the other end of my stdout!
[PASS] Success! You have satisfied all execution requirements.
pwn.college{AyfAlgu1S14DvRR0k-49RrYW62m.QX5EDO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{AyfAlgu1S14DvRR0k-49RrYW62m.QX5EDO0wyNwEzNzEzW}
```

### Notes:

- `|` only connects stdout of the left command to stdin of the right. Stderr from `/challenge/run` will still go to your terminal unless redirected (e.g., `2>/dev/null`).

# Challenge 8 Grepping errors

`/challenge/run` floods stderr (fd 2) with a lot of output and one of those error-lines contains the flag. The pipe `|` only sends stdout (fd 1) to the next program, so to search stderr you must first redirect stderr into stdout and then pipe. Your goal: get the stderr stream into `grep` and print the flag.

## Solution:

- File descriptors: 0=stdin, 1=stdout, 2=stderr. `|` connects fd 1 of the left command to stdin of the right command.
- To send stderr through the pipe you must make fd 2 refer to the same destination as fd 1. The usual form is `2>&1`. Putting that before the pipe causes the combined stream (stdout+stderr) to be piped. Example: `/challenge/run 2>&1 | grep ...` will let grep see both streams.

#### Commands run: 

```sh
hacker@piping~grepping-errors:~$ /challenge/run 2>&1 | grep pwn.college
[INFO] WELCOME! This challenge makes the following asks of you:
[INFO] - the challenge checks for a specific process at the other end of stderr : grep
[INFO] - the challenge will output a reward file if all the tests pass : /challenge/.data.txt

[HYPE] ONWARDS TO GREATNESS!

[INFO] This challenge will perform a bunch of checks.
[INFO] If you pass these checks, you will receive the /challenge/.data.txt file.

[TEST] You should have redirected my stderr to another process. Checking...
[TEST] Performing checks on that process!

[INFO] The process' executable is /nix/store/8b4vn1iyn6kqiisjvlmv67d1c0p3j6wj-gnugrep-3.11/bin/grep.
[INFO] This might be different than expected because of symbolic links (for example, from /usr/bin/python to /usr/bin/python3 to /usr/bin/python3.8).
[INFO] To pass the checks, the executable must be grep.

[PASS] You have passed the checks on the process on the other end of my stderr!
[PASS] Success! You have satisfied all execution requirements.
pwn.college{AZWX1NjBUvrV6XZLErBMoofZEAl.QX1ATO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{AZWX1NjBUvrV6XZLErBMoofZEAl.QX1ATO0wyNwEzNzEzW}
```

### Notes:

- If you want only stderr to be grepped (stdout suppressed), do the fd trick in the correct order so stderr winds up in the pipe while stdout goes elsewhere. The canonical ordering is:

`2>&1 1>/dev/null` - first duplicate stderr onto current stdout, then send stdout to `/dev/null`. After these two redirections the pipe receives what used to be stderr only. So: `/challenge/run 2>&1 1>/dev/null | grep ....`

# Challenge 9 Filtering with grep -v

`/challenge/run` prints the real flag to stdout but also prints 1000+ decoy flags that contain the word `DECOY`. Use `grep -v` (invert match) to remove all lines containing `DECOY` and reveal the real `pwn.college{...}` flag.

## Solution:

- The program prints many lines; the decoys are identifiable because they contain the substring `DECOY`.
- `grep -v DECOY` removes every line that contains `DECOY`, leaving only lines that do not contain that substring.
- The real flag follows the usual format `pwn.college{...}`. After filtering decoys.

#### Commands run: 

```sh
hacker@piping~filtering-with-grep-v:~$ /challenge/run | grep -v DECOY
pwn.college{gHKJAXlnZAa4pqonKsbEYUguI1b.0FOxEzNxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{gHKJAXlnZAa4pqonKsbEYUguI1b.0FOxEzNxwyNwEzNzEzW}
```

### Notes:

- `grep -v` filters out any line containing the pattern. The match is case-sensitive by default (DECOY ≠ decoy).
- Common mistake: running `grep DECOY` instead of `grep -v DECOY`, that returns decoys, not the real flag.

# Challenge 10 Duplicating piped data with tee

`/challenge/pwn` must be piped into `/challenge/college`. Intercept the data in the middle so you can see what `/challenge/pwn` sends while still passing it on to `/challenge/college`. Use `tee` to duplicate the stream.

## Solution:

- `tee` writes piped stdin to files and to stdout, so anything after `tee` in the pipeline still receives the original stream.
- Insert `tee` between `/challenge/pwn` and `/challenge/college` to save a copy to a file you can inspect while the pipeline runs.

#### Commands run: 

```sh
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn | tee /tmp/out | /challenge/college
Processing...
The input to 'college' does not contain the correct secret code! This code 
should be provided by the 'pwn' command. HINT: use 'tee' to intercept the 
output of 'pwn' and figure out what the code needs to be.
hacker@piping~duplicating-piped-data-with-tee:~$ cat /tmp/out
Usage: /challenge/pwn --secret [SECRET_ARG]

SECRET_ARG should be "Q2Udn-AE"
hacker@piping~duplicating-piped-data-with-tee:~$ /challenge/pwn --secret Q2Udn-AE | /challenge/college
Processing...
Correct! Passing secret value to /challenge/college...
Great job! Here is your flag:
```

## Flag: 

```
pwn.college{Q2Udn-AEPc9UWaq5MdnrhONRV7U.QXxITO0wyNwEzNzEzW}
```

### Notes:

- `tee` duplicates stdin: one copy goes to the file(s) you list, the other goes to stdout (so the next command receives it).
- `/challenge/pwn` (the file) is usually a binary (or script). `cat /challenge/pwn` just dumps that file’s raw bytes to your terminal, mostly non-human garbage for binaries.
- Running `/challenge/pwn` executes code: that process may print readable text (the flag, status lines, etc.) to stdout.

# Challenge 11 Process substitution for input

Compare the output of two programs using process substitution. `/challenge/print_decoys` prints many decoy flags; `/challenge/print_decoys_and_flag` prints the same decoys plus the real flag. Use process substitution with `diff` to find the real flag.

## Solution:

- We want to compare outputs of two commands, not files. Saving to files works, but process substitution is cleaner.
- `<(command)` runs command and exposes its stdout as a filename (a named pipe like `/dev/fd/63`) that programs expecting files can read.
- `diff <(cmd1) <(cmd2)` compares the two generated streams line-by-line.
- The real flag will appear only in the output of `/challenge/print_decoys_and_flag`. `diff` will show differences.
- Final approach: run `diff` on the two process substitutions and filter the `diff` output for the `pwn.college{...}` pattern.

#### Commands run: 

```sh
hacker@piping~process-substitution-for-input:~$ diff <(/challenge/print_decoys) <(/challenge/print_decoys_and_flag)
63a64
> pwn.college{8EkTO6xH7Wv8rH-JKfW4xY8WI6J.0lNwMDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{8EkTO6xH7Wv8rH-JKfW4xY8WI6J.0lNwMDOxwyNwEzNzEzW}
```

### Notes:

- Process substitution uses named pipes (`/dev/fd/*`). You can `echo <(cmd)` to see the path the shell creates.

# Challenge 12 Writing to multiple programs

Run `/challenge/hack` and duplicate its output as input to both `/challenge/the` and `/challenge/planet`. Use output process substitution `>(command)` to achieve this, possibly with `tee`.

## Solution:

- `tee` can duplicate standard input to multiple outputs: files or standard output.
- Output process substitution `>(command)` makes a command appear like a file; writing to it sends data to the command’s stdin.
- Combining tee and output process substitution lets us send the same data to multiple commands.

#### Commands run: 

```sh
hacker@piping~writing-to-multiple-programs:~$ /challenge/hack | tee >(/challenge/the) >(/challenge/planet)
Congratulations, you have duplicated data into the input of two programs! Here 
is your flag:
```

## Flag: 

```
pwn.college{EeaPdIOMaC62Fwgpokd8qViEtSc.QXwgDN1wyNwEzNzEzW}
```

### Notes:

- `>(command)` behaves like a temporary file; writing to it sends data to the command’s stdin.
- You can combine multiple `>(command)`s with `tee` to write to several commands simultaneously.

# Challenge 13 Split-piping stderr and stdout

Run `/challenge/hack` which writes to both stdout and stderr. Redirect its stdout into `/challenge/planet` and its stderr into `/challenge/the`, keeping the streams separate (do not mix them). Use output process substitution `>(command)` together with `>` and `2>` to accomplish this.

## Solution:

- We need two distinct destinations: one for file descriptor 1 (stdout) and one for file descriptor 2 (stderr).
- Output process substitution `>(cmd)` makes `cm`d look like a writable file: writing to that path sends data to `cmd`'s stdin.
- So redirect stdout with `> >( /challenge/planet )` and stderr with `2> >( /challenge/the )`.
- Order of the redirections doesn't matter much here, each fd is handled explicitly. Avoid `2>&1` (that merges streams) or piping `|` alone (it only connects stdout).
- Final command runs `/challenge/hack` once and routes its streams separately to the two programs

#### Commands run: 

```sh
hacker@piping~split-piping-stderr-and-stdout:~$ /challenge/hack > >(/challenge/planet) 2> >(/challenge/the)
Congratulations, you have learned a redirection technique that even experts 
struggle with! Here is your flag:
```

## Flag: 

```
pwn.college{k3zPyEj4L_Cz-qQho3QuCNI2xDd.QXxQDM2wyNwEzNzEzW}
```

### Notes:

- This is explicit fd-level redirection, `>` addresses fd1, `2>` addresses fd2. Use `>(cmd)` when you want a command to receive the written data.

# Challenge 14 Named pipes

Create a named `FIFO` at `/tmp/flag_fifo` and redirect the stdout of `/challenge/run` into it. Because `FIFOs` block until both reader and writer are present, you must ensure something reads from the `FIFO` so `/challenge/run` can succeed and write the flag into the pipe.

## Solution:

- A `FIFO` (created with `mkfifo`) behaves like a file path you can read/write, but writes block until a reader opens the `FIFO`, and reads block until a writer opens it.
- Create the `FIFO`, start a reader on it (for example `cat /tmp/flag_fifo`) so the pipe is open for reading, then run `/challenge/run > /tmp/flag_fifo` to send the flag into the FIFO.
- You can do this in two terminals (recommended) or start the reader in the background from a single terminal.

#### Commands run: 

```sh
saumya@LAPTOP-9BJ63QM4:~$ ssh hacker@dojo.pwn.college
Connected!
hacker@piping~named-pipes:~$ mkfifo /tmp/flag_fifo
hacker@piping~named-pipes:~$ /challenge/run > /tmp/flag_fifo
You're successfully redirecting /challenge/run to a FIFO at /tmp/flag_fifo!
Bash will now try to open the FIFO for writing, to pass it as the stdout of
/challenge/run. Recall that operations on FIFOs will *block* until both the
read side and the write side is open, so /challenge/run will not actually be
launched until you start reading from the FIFO!

saumya@LAPTOP-9BJ63QM4:/mnt/c/Users/aarya$ ssh hacker@dojo.pwn.college
Connected!
hacker@piping~named-pipes:~$ cat /tmp/flag_fifo
You've correctly redirected /challenge/run's stdout to a FIFO at
/tmp/flag_fifo! Here is your flag:
```

## Flag: 

```
pwn.college{o0AX5prVfLruTUKC7PbDLvVcWoc.01MzMDOxwyNwEzNzEzW}
```

### Notes:

- The simplest, least surprising workflow is the two-terminal approach: start `cat /tmp/flag_fifo` in one terminal, then run the writer in another. No race conditions, and it's easy to read the output.
- Single-terminal trick: start the reader in the background (`(cat /tmp/flag_fifo &)`) before running the writer, that prevents the writer from blocking forever.