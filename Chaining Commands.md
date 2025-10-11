# Challenge 1 Chaining with Semicolons

Run `/challenge/pwn` and then immediately run `/challenge/college` on a single command line by chaining the two commands with a semicolon (`;`). The level checks that you invoked both in sequence.

## Solution:

- The semicolon `;` causes the shell to run the first command, wait for it to finish, then run the second command regardless of the first command’s exit status.
- Chain `/challenge/pwn` alongwith `/challenge/college` using a `;`

#### Commands run: 

```sh
hacker@chaining~chaining-with-semicolons:~$ /challenge/pwn; /challenge/college
Yes! You chained /challenge/pwn and /challenge/college! Here is your flag:
pwn.college{YOUnjpeETSFC2e-md0Lh46I0uDe.QX1UDO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{YOUnjpeETSFC2e-md0Lh46I0uDe.QX1UDO0wyNwEzNzEzW}
```

### Notes:

- `;` always runs the next command after the previous finishes (no exit-status checking).
- `&&` runs the next command only if the previous returned exit status `0`; `||` runs it only if the previous returned `non-zero`.

# Challenge 2 Building on Success

You must run `/challenge/first-success` and `/challenge/second` so that the second program runs only if the first one succeeds. Use the shell `&&` operator (`AND`). Running them separately won't give the flag; chaining with `&&` will.

## Solution:

- `cmd1 && cmd2` runs `cmd2` only when `cmd1` exits with code `0` (success). If `cmd1` fails (non‑zero exit), `cmd2` is skipped.
- First, run each binary separately to observe their behavior and exit codes: `/challenge/first-success` then `/challenge/second`.
- Confirm `/challenge/first-success` exits with code `0` by checking `echo $?` immediately after it. If it prints `0`, chaining will allow the second to run.
- Finally run them together with `&&`: `/challenge/first-success && /challenge/second`. That exact form is what the level checks for.

#### Commands run: 

```sh
hacker@chaining~building-on-success:~$ /challenge/first-success
hacker@chaining~building-on-success:~$ /challenge/second
Error: /challenge/first-success must be successfully chained with 
/challenge/second using &&
hacker@chaining~building-on-success:~$ /challenge/first-success && /challenge/second
Nice chaining! Flag: pwn.college{0QNYFqZ0ip_GytNZOvzD0nF5ZvU.0lM0MDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{0QNYFqZ0ip_GytNZOvzD0nF5ZvU.0lM0MDOxwyNwEzNzEzW}
```

### Notes:

- `&&` checks the exit status (`0` == success). Using `;` will run the second regardless, wrong for this level.

# Challenge 3 Handling Failure

You must run `/challenge/first-failure` and `/challenge/second` so that the second program runs only if the first one fails. Use the shell `||` operator (OR). Running them separately won’t guarantee the flag; chaining with `||` ensures the second command runs on failure of the first.

## Solution:

- `/challenge/first-failure || /challenge/second` runs the second command only when the first exits with a non-zero code (failure).
- First, run each command separately to see how they behave and what their exit codes are.
- Confirm `/challenge/first-failure` fails by running `echo $?` immediately afterward; a non-zero exit means the second command will execute when chained with ``.
- Finally, run the exact chained form to get the flag.

#### Commands run: 

```sh
hacker@chaining~handling-failure:~$ /challenge/first-failure || /challenge/second
Nice chaining! Flag: pwn.college{cHOXk7Etqi3dViZQbmXLV3rJFpq.01M0MDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{cHOXk7Etqi3dViZQbmXLV3rJFpq.01M0MDOxwyNwEzNzEzW}
```

### Notes:

- `||` checks the exit status (non-zero = failure). Using `&&` would run the second command only on success — wrong for this level.

# Challenge 4 Your First Shell Script

Create a shell script file named `x.sh` that runs `/challenge/pwn` and then `/challenge/college` in order. Execute the script with `bash x.sh` so both commands run. The level checks that you invoked both programs from a script.

## Solution:

- Shell scripts are plain text files containing the commands you would otherwise type interactively.
- Put the two commands on separate lines in `x.sh` so the shell reads and executes them in order.
- Use absolute paths in the script to avoid $PATH confusion (`/challenge/pwn` and `/challenge/college`).
- Run the script with `bash x.sh` (this runs a new shell that reads commands from the file).
- Verify both commands ran and produced their outputs; the level will then give the flag.

#### Commands run: 

```sh
hacker@chaining~your-first-shell-script:~$ echo /challenge/pwn > x.sh
hacker@chaining~your-first-shell-script:~$ echo /challenge/college >> x.sh
hacker@chaining~your-first-shell-script:~$ bash x.sh
Great job, you've written your first shell script! Here is the flag:
pwn.college{09lywWaPP4kb-NGwRM0DgJKN0UC.QXxcDO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{09lywWaPP4kb-NGwRM0DgJKN0UC.QXxcDO0wyNwEzNzEzW}
```

### Notes:

- `bash x.sh` runs the script even if it is not executable; you do not need to set the execute bit.

# Challenge 5 Redirecting Script Output

This level asks you to create a shell script that runs `/challenge/pwn` and then `/challenge/college` (in that order), and pipe the combined output of that script into a single invocation of `/challenge/solve`. The script should be run with `bash` or made executable and run as `./x.sh`, and the final check will be satisfied by piping the script’s stdout into `/challenge/solve`.

## Solution:

- Create a shell script that runs the two required programs in sequence so their outputs appear one-after-the-other on stdout.
- Ensure no extra interactive input is required; the commands should output directly to stdout.
- Execute the script with `bash x.sh` (or make it executable and run `./x.sh`) while piping its stdout into a single call to `/challenge/solve` so it receives the combined output stream.
- Confirm the pipeline exactly as: `bash x.sh | /challenge/solve`.
- Avoid splitting the commands into separate pipes (e.g., `/challenge/pwn | /challenge/solve` and `/challenge/college | /challenge/solve`), that will fail because `/challenge/solve` must get all output at once.

#### Commands run: 

```sh
hacker@chaining~redirecting-script-output:~$ cat x.sh
/challenge/pwn
/challenge/college
hacker@chaining~redirecting-script-output:~$ bash x.sh | /challenge/solve
Correct! Here is your flag:
pwn.college{oqDpn1xvbFFm-YbhGSgoYI5N1-6.QX4ETO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{oqDpn1xvbFFm-YbhGSgoYI5N1-6.QX4ETO0wyNwEzNzEzW}
```

### Notes:

- Common mistake: piping each command separately, you must pipe the single script output into one `/challenge/solve`.

# Challenge 6 Executable Shell Scripts

This level asks you to write a shell script that invokes `/challenge/solve`, make that script executable (so you don't have to run it with `bash`), and then run it directly via a relative or absolute path (for example `./script.sh`). The level checks that you executed the script itself (not `bash script.sh`) and that `/challenge/solve` received the script's stdout.

## Solution:

- Create a minimal shell script whose only job is to run `/challenge/solve` so the program receives whatever the script prints (and/or simply runs the solver).
- Make the file executable with `chmod +x script.sh` so the kernel will use the shebang (or the calling shell) to run it without bash as an explicit argument.
- Run the script by path (e.g. `./script.sh` or `/home/hacker/script.sh`) rather than bash script.sh to satisfy the “executable invocation” requirement.
- If `/challenge/solve` needs input from the script (stdout), ensure your script writes that input or pipes it into `/challenge/solve` so it receives the combined stream.
- If the check expects exact stdout formatting, avoid extra `echo` lines or trailing newlines beyond what the level expects.

#### Commands run: 

```sh
hacker@chaining~executable-shell-scripts:~$ nano script.sh
hacker@chaining~executable-shell-scripts:~$ chmod +x script.sh
hacker@chaining~executable-shell-scripts:~$ ./script.sh
Congratulations on your shell script execution! Your flag:
pwn.college{QCaOcHH3hOHYl8-9pUSpCC8memZ.QX0cjM1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{QCaOcHH3hOHYl8-9pUSpCC8memZ.QX0cjM1wyNwEzNzEzW}
```

### Notes:

- If you must run `/challenge/solve` from within the script and the level checks that `/challenge/solve` was executed exactly once, ensure you only call it once from the script.

# Challenge 7 Understanding Shebangs

This level asks you to create the file `/home/hacker/solve.sh` with a proper shebang as the very first line (for example `#!/bin/bash`), make it executable (`chmod +x /home/hacker/solve.sh`), ensure the script prints exactly `hack the planet`, and then run `/challenge/run` to verify the script works.

## Solution:

- Create the script file at `/home/hacker/solve.sh` with the shebang on the very first line so the kernel invokes the correct interpreter.
- Ensure the script prints exactly `hack the planet` (no extra leading/trailing spaces; trailing newline is normally fine).
- Make the script executable using `chmod +x /home/hacker/solve.sh` so you can run it directly (e.g. `./solve.sh` from /home/hacker).
- Invoke the verifier with `/challenge/run` which will call your executable script and check its output.
- If you need to avoid the trailing newline, use `printf 'hack the planet'` instead of `echo`.

#### Commands run: 

```sh
hacker@chaining~understanding-shebangs:~$ nano solve.sh
hacker@chaining~understanding-shebangs:~$ chmod +x solve.sh
hacker@chaining~understanding-shebangs:~$ ./solve.sh
hack the planet
hacker@chaining~understanding-shebangs:~$ /challenge/run
Testing your script...
Perfect! Your flag:
Flag: pwn.college{0znTDk34DbQ_MmGft6AnQqg2IKn.0VOzMDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{0znTDk34DbQ_MmGft6AnQqg2IKn.0VOzMDOxwyNwEzNzEzW}
```

### Notes:

- Make sure there are no blank lines or spaces before the shebang; the shebang must be the first bytes of the file (`#!/bin/bash`).

# Challenge 8 Scripting with Arguments

This level asks you to create the script file `/home/hacker/solve.sh` which accepts two command-line arguments and prints them in reverse order (second argument first, then the first). After making the script executable (`chmod +x /home/hacker/solve.sh`) verify it by running the challenge harness `/challenge/run` to receive the flag.

## Solution:

- Use a shebang on the first line (for example `#!/bin/bash`) so the script runs under bash when executed.
- Access positional parameters via `$1` for the first argument and `$2` for the second.
- Print the arguments in reverse order using either `echo "$2 $1"` or `printf '%s %s\n' "$2" "$1"` to ensure exact formatting.
- Make the file executable with `chmod +x /home/hacker/solve.sh` and then run the verifier with `/challenge/run`.
- Test manually from `/home/hacker` with `./solve.sh pwn college` (or with `bash /home/hacker/solve.sh pwn college` while debugging) to confirm output is `college pwn`.

#### Commands run: 

```sh
hacker@chaining~scripting-with-arguments:~$ nano solve.sh
hacker@chaining~scripting-with-arguments:~$ chmod +x solve.sh
hacker@chaining~scripting-with-arguments:~$ ^C
hacker@chaining~scripting-with-arguments:~$ ./solve.sh pwn college
college pwn
hacker@chaining~scripting-with-arguments:~$ /challenge/run
Correct! Your script properly reversed the arguments.
Here's your flag:
pwn.college{oqaNSZHst43QxgHtrnIK2bdWSKS.0VNzMDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{oqaNSZHst43QxgHtrnIK2bdWSKS.0VNzMDOxwyNwEzNzEzW}
```

### Notes:

- If the checker is strict about extra input or errors, avoid printing additional debugging messages to stdout or stderr.

# Challenge 9 Scripting with Conditionals

This level asks you to create the script file `/home/hacker/solve.sh` which accepts a single command-line argument and prints `college` only if that argument is exactly `pwn`; for any other input the script must produce no output. After making the script executable (`chmod +x /home/hacker/solve.sh`) verify it by running the challenge harness `/challenge/run`.

## Solution:

- Use a shebang on the first line (for example `#!/bin/bash`) so the script runs under bash when executed.
- Read the first positional parameter via `$1` and compare it to the literal string `pwn` using the test/ construct: `if [ "$1" = "pwn" ] ; then`.
- Print exactly `college` and nothing else when the condition matches; otherwise do nothing (no outputs, no error messages).
- Make the script executable with `chmod +x /home/hacker/solve.sh` and run the verifier with `/challenge/run`.
- To debug locally, you can run `./solve.sh pwn` (expected output: college) and `./solve.sh foo` (expected output: nothing).

#### Commands run: 

```sh
hacker@chaining~scripting-with-conditionals:~$ nano solve.sh
hacker@chaining~scripting-with-conditionals:~$ bash solve.sh pwn
college
hacker@chaining~scripting-with-conditionals:~$ bash solve.sh jjjsjsj
hacker@chaining~scripting-with-conditionals:~$ /challenge/run
Correct! Your script properly handles all the conditions.
Here's your flag:
pwn.college{Mnjk-Z9mtNGAXPEiKtlMpexCDrT.0lNzMDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{Mnjk-Z9mtNGAXPEiKtlMpexCDrT.0lNzMDOxwyNwEzNzEzW}
```

### Notes:

- Quoting `"$1"` preserves arguments that contain spaces and avoids empty-string pitfalls.

# Challenge 10 Scripting with Default Cases

This level asks you to create the script file `/home/hacker/solve.sh` which accepts a single command-line argument and prints `college` if the argument is exactly `pwn`, otherwise prints `nope`. After writing the script and making it executable (`chmod +x /home/hacker/solve.sh`), run the verifier `/challenge/run` to get your flag.

## Solution:

- Use a shebang as the first line (for example `#!/bin/bash`) so the script runs under bash when executed.
- Read the first positional parameter via `$1` and compare it to the literal string `pwn` using the test/ construct with an `if` / `else` / `fi` block.
- Print exactly `college` when the argument equals `pwn`, otherwise print exactly `nope` and nothing else.
- Make the script executable with `chmod +x /home/hacker/solve.sh` and run the verifier with `/challenge/run`.
- Test locally with `./solve.sh pwn` (expected college) and `./solve.sh hack` (expected nope) before running the harness.

#### Commands run: 

```sh
hacker@chaining~scripting-with-default-cases:~$ nano /home/hacker/solve.sh
hacker@chaining~scripting-with-default-cases:~$ bash /home/hacker/solve.sh pwn
college
hacker@chaining~scripting-with-default-cases:~$ bash solve.sh heheehe
nope
hacker@chaining~scripting-with-default-cases:~$ /challenge/run
Correct! Your script properly handles the if/else conditions.
Here's your flag:
pwn.college{kKCgbwy9y15EXflypWbxazX2ksC.01NzMDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{kKCgbwy9y15EXflypWbxazX2ksC.01NzMDOxwyNwEzNzEzW}
```

### Notes:

- Common mistake: missing the shebang or having blank lines before, it the shebang must be the very first bytes if the script will be executed directly.

# Challenge 11 Scripting with Multiple Conditions

This level asks you to create the script file `/home/hacker/solve.sh` which accepts a single command-line argument and prints `the planet` when the argument is `hack`, `college` when the argument is `pwn`, `linux` when the argument is `learn`, and prints `unknown` for any other input. Make the script executable (`chmod +x /home/hacker/solve.sh`) and run the verifier `/challenge/run` when you're ready.

## Solution:

- Use a shebang as the first line (for example `#!/bin/bash`) so the script runs under bash when executed.
- Read the first positional parameter via `$1` and compare it using an `if` / `elif` / `else` / `fi` chain, remembering spaces around [ and ].
- Print the exact strings required (the planet, college, linux, or unknown) with `printf '%s\n' ...` to ensure predictable output.
- Make the file executable with `chmod +x /home/hacker/solve.sh` and run the verifier with `/challenge/run`.
- Test locally with `./solve.sh hack`, `./solve.sh pwn`, `./solve.sh learn`, and `./solve.sh foo` to confirm results before running the harness.

#### Commands run: 

```sh
hacker@chaining~scripting-with-multiple-conditions:~$ nano solve.sh
hacker@chaining~scripting-with-multiple-conditions:~$ bash solve.sh hack
the planet
hacker@chaining~scripting-with-multiple-conditions:~$ /challenge/run
Correct! Your script properly handles all the conditions with elif.
Here's your flag:
pwn.college{ALJ0imgfJ5wSnMiuozE1hX0I8Md.0FOzMDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{ALJ0imgfJ5wSnMiuozE1hX0I8Md.0FOzMDOxwyNwEzNzEzW}
```

### Notes:

- Remember to include spaces around [ and ] in test expressions (for example `if [ \"$1\" = \"hack\" ]`).

# Challenge 12 Reading Shell Script

Inspect the challenge runner script `/challenge/run` to find the hardcoded password, then submit that password non-interactively to the verifier to get the flag.

## Solution:

- I opened the script to read its contents using `cat /challenge/run` and looked for password-related logic.
- I identified the exact string the script compares against: `hack the PLANET` (note: case-sensitive).
- I submitted the password non-interactively so the verifier would receive it as if I typed it, using `echo 'hack the PLANET' | /challenge/run`.
- I verified the verifier prints CORRECT! Your flag: and then the contents of `/flag`.

#### Commands run: 

```sh
hacker@chaining~reading-shell-scripts:~$ cat /challenge/run
#!/opt/pwn.college/bash

read GUESS
if [ "$GUESS" == "hack the PLANET" ]
then
        echo "CORRECT! Your flag:"
        cat /flag
else
        echo "Read the /challenge/run file to figure out the correct password!"
fi
hacker@chaining~reading-shell-scripts:~$ echo 'hack the PLANET' | /challenge/run
CORRECT! Your flag:
pwn.college{0AITn-xkU5X6wUHVegqXwBpbdjL.0lMwgDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{0AITn-xkU5X6wUHVegqXwBpbdjL.0lMwgDOxwyNwEzNzEzW}
```

### Notes:

- The script contained the check if [ "$GUESS" == "hack the PLANET" ], so the correct secret is exactly `hack the PLANET`.
