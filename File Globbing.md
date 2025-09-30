# Challenge 1 Matching with *

Starting from your home directory, change directory to /challenge, but use globbing so the argument you pass to cd is at most four characters. Once you're there, run /challenge/run to get the flag.

## Solution:

- The `*` glob matches any sequence of characters (except `/` and `leading` .).
- We must pass an argument to `cd` that is no more than four characters long (when typed). A simple literal that satisfies that and expands to `/challenge` is `/ch*`, that is exactly four characters: `/`, `c`, `h`, `*`.
- `cd /ch*` will expand to `/challenge`, so the shell will `cd` into the correct directory while obeying the four-character constraint.
- After changing directory, running `/challenge/run` (absolute path) executes the challenge binary to print the flag.

#### Commands run: 

```sh
hacker@globbing~matching-with-:~$ cd /ch*
hacker@globbing~matching-with-:/challenge$ /challenge/run
You ran me with the working directory of /challenge! Here is your flag:
```

## Flag: 

```
pwn.college{cqFRK2hDgzlkTDN9j1OIs4-q5mm.QXxIDO0wyNwEzNzEzW}
```

### Notes:

- Alternative globs that are ≤4 characters (when typed) could work only if they uniquely match `/challenge`. `/ch*` is safe and clear.
- Common mistake: attempting `cd challenge` (no leading `/`) from home won't reach `/challenge`. The requirement was to start from home and reach the root-level `/challenge`.

# Challenge 2 Matching with ?

Starting from your home directory, change directory to `/challenge`, but use the `?` character instead of the c and l characters in the argument to `cd`. Once you're there, run `/challenge/run` to get the flag.

## Solution:

- `?` matches exactly one character (anything except `/` or a leading `.`)
- The target path is `/challenge`. We must replace the c and both l characters with `?`.
- Replacing those characters yields the pattern `/?ha??enge` (`/` + `?` for c + h + a + `?` for first l + `?` for second l + e + n + g + e).
- When the shell expands `/?ha??enge` it will match `/challenge`, so `cd /?ha??enge` will change into `/challenge` (assuming no other root-level entry collides).
= After cding, run `/challenge/run` to print the flag.

#### Commands run: 

```sh
hacker@globbing~matching-with-:~$ cd /?ha??enge
hacker@globbing~matching-with-:/challenge$ /challenge/run
You ran me with the working directory of /challenge! Here is your flag:
```

## Flag: 

```
pwn.college{QNyKQ9aRsENFBmEfniU-6FVqra_.QXyIDO0wyNwEzNzEzW}
```

### Notes:

- Use exactly one `?` for each character you want to replace. To replace c and both l characters in `/challenge`, the correct pattern is `/?ha??enge`.
- Common mistakes: Using the wrong number of `?` (e.g. `/?ha?enge`, that only replaces one l instead of two). Count characters!

# Challenge 3 Matching with []

Change your working directory to `/challenge/files` and run `/challenge/run` with a single argument that uses a bracket glob to match `file_b`, `file_a`, `file_s`, and `file_h`.

## Solution:

- Square brackets `[]` match exactly one character from the set inside the brackets.
- We need to match `file_a`, `file_b`, `file_s`, and `file_h`. The varying character is the single letter after the underscore: b, a, s, h.
- A single bracket expression that matches those is `[bash]`.
- So the pattern `file_[bash]` will expand to `file_a` `file_b` `file_h` `file_s` (shell sorts matches) when run from /challenge/files.

#### Commands run: 

```sh
hacker@globbing~matching-with-:~$ cd /challenge/files
hacker@globbing~matching-with-:/challenge/files$ /challenge/run file_[bash]
You got it! Here is your flag!
```

## Flag: 

```
pwn.college{wREZneT2FYEL6Z8kq6arOmFaXLf.QXzIDO0wyNwEzNzEzW}
```

### Notes:

- Use one character class for the single differing character: `file_[bash]` matches `file_a`, `file_b`, `file_s`, and `file_h`.
- If there are other files like `file_x` present, they won’t be matched unless x is in the class. That’s the advantage, very specific selection.

# Challenge 4 Matching paths with []

Starting from your home directory, run `/challenge/run` with a single argument that bracket-globs into the absolute paths for `file_b`, `file_a`, `file_s`, and `file_h` which are in `/challenge/files`

## Solution:

- We need absolute paths to the four files in `/challenge/files`. The only varying character is the single letter after the underscore, so a single bracket class covers them: `[bash]`.
- Use the bracket glob in the path portion so it expands to `/challenge/files/file_a`, `/challenge/files/file_b`, `/challenge/files/file_h`, and `/challenge/files/file_s`.
- The shell expands the glob before calling `/challenge/run`, so pass `/challenge/files/file_[bash]` as the single argument on the command line.

#### Commands run: 

```sh
hacker@globbing~matching-paths-with-:~$ echo /challenge/files/file_[bash]
/challenge/files/file_a /challenge/files/file_b /challenge/files/file_h /challenge/files/file_s
hacker@globbing~matching-paths-with-:~$ /challenge/run /challenge/files/file_[bash]
You got it! Here is your flag!
```

## Flag: 

```
pwn.college{UnCT5-8wGNtdKnSpZh1tzJXXATu.QX0IDO0wyNwEzNzEzW}
```

### Notes:

- Use the full path pattern `/challenge/files/file_[bash]` so the shell expands to absolute paths.

# Challenge 5 Multiple globs

Change directory to `/challenge/files` and run `/challenge/run`, providing a single argument: a short (3 characters or less) globbed word containing two `*` globs that matches every filename that contains the letter `p`.

## Solution:

- You need a 3-character token with two `*` characters that matches any filename containing `p`.
- `*p*` does exactly that: match any (possibly empty) prefix, the letter p, then any (possibly empty) suffix — so any filename containing p anywhere will be matched.
- Provide `*p*` as the single argument on the command line; the shell expands it to all matching filenames before calling `/challenge/run`.

#### Commands run: 

```sh
hacker@globbing~multiple-globs:~$ cd /challenge/files
hacker@globbing~multiple-globs:/challenge/files$ /challenge/run *p*
You got it! Here is your flag!
```

## Flag: 

```
pwn.college{Ap6Ijd9flOU25j6-jEFKjh9rtck.0lM3kjNxwyNwEzNzEzW}
```

### Notes:

- `*p*` is exactly 3 characters (two `*` and one `p`) and matches any filename containing p anywhere.
- Common mistakes: Using more than 3 characters (the challenge constraint),`*p*` is the minimal correct token.

# Challenge 6 Mixing globs

From your home directory, change into `/challenge/files` and run `/challenge/run` with a single short (≤6 chars) bracket-glob that expands exactly to the files `challenging`, `educational`, and `pwning`.

## Solution:

- We need a single glob token (≤6 characters) that uses `[]` and matches only the three target filenames.
- The three targets begin with `c`, `e`, and `p` respectively. No other files in the shown listing start with those letters.
- A bracket that matches those first letters, followed by `*` to allow the rest of the name, will match just those files.
- The minimal, correct glob is: `[cep]*`, this is 6 characters long ([,c,e,p,],*) and expands to `challenging` `educational` `pwning`.

#### Commands run: 

```sh
hacker@globbing~mixing-globs:~$ cd /challenge/files
hacker@globbing~mixing-globs:/challenge/files$ /challenge/run [cep]*
You got it! Here is your flag!
```

## Flag: 

```
pwn.college{I0dCEzjHQI2_1mlmynv8d08ohoq.QX1IDO0wyNwEzNzEzW}
```

### Notes:

- `[cep]*` anchors the bracket at the start of the filename: match names beginning with c, e, or p.
- `*[cep]` would be totally different — it would match names ending with c, e, or p. Position matters.

# Challenge 7 Exclusionary globbing

Go to `/challenge/files` and run `/challenge/run` with a glob that matches all files that don’t start with `p`, `w`, or `n`. Use the bracket-negation feature (`[!...]` or `[^...]`).

## Solution:

- In bracket globs, putting `!` (or `^` in newer bash) at the start of the bracket means “not these characters.”
- We need all files whose first letter is not p, w, or n.
- That’s expressed as `[!pwn]*` (or `[^pwn]*`).
- The `*` after the bracket ensures we match the rest of the filename, regardless of length.
- So the glob `[!pwn]*` expands to all files in the current directory whose names do not start with p, w, or n.

#### Commands run: 

```sh
hacker@globbing~exclusionary-globbing:~$ cd /challenge/files
hacker@globbing~exclusionary-globbing:/challenge/files$ /challenge/run [!pwn]*
You got it! Here is your flag!
```

## Flag: 

```
pwn.college{kLSczA-r1rLizc1xY2S1JNB72TU.QX2IDO0wyNwEzNzEzW}
```

### Notes:

- Negated classes: `[!pwn]` means "any one character that is not p, w, or n." `[^pwn]` is equivalent, but not supported in some older shells.

# Challenge 8 Tab completion

The flag file was placed at `/challenge/pwncollege`, but the challenge prevents you from typing the full filename directly. You must use shell tab‑completion so the shell expands the file name for you and then `cat` it to read the flag. The goal: open the file using tab completion.

## Solution:

- The shell's tab key triggers filename completion using the current completion settings (readline / bash completion).
- You can confirm what exists in /challenge with ls /challenge. If the directory lists a confusing or hidden entry, tab completion will still work because it reads the filesystem.
- To use completion, start typing the path and the start of the filename, then press Tab. If there’s a single unambiguous match, the shell will expand it.
- Here the file begins with pwn..., so typing `cat /challenge/pwn` then pressing Tab will expand the full filename to `cat /challenge/pwncollege` and you can press Enter to run it.

#### Commands run: 

```sh
hacker@globbing~tab-completion:~$ cat /challenge/pwncollege​ (pwn<TAB>)
```

## Flag: 

```
pwn.college{w8UGbPj4xnxe7LfYHpJmdYMIluY.0FN0EzNxwyNwEzNzEzW}
```

### Notes:

- Quoting prevents normal filename completion. Do not start a quoted string if you want completion to run.
- Don’t rely on wildcards like `*` when you can safely tab-complete; wildcards can accidentally expand to many files (danger with `rm`). Tab completion helps avoid that mistake.

# Challenge 9 Multiple options for tab completion

A directory `/challenge/files` contains many files whose names begin with `pwncollege....` Starting from a short prefix (for example `/challenge/files/p`), use shell tab‑completion to navigate the multiple completion possibilities and open the flag file. The twist: when there are multiple matches, shells behave differently, some stop at the common prefix and list options on a second Tab, others cycle through candidates. Use interactive tab completion to get the full filename and `cat` it to read the flag.

## Solution:

- Start typing the path and an initial prefix, for example `cat /challenge/files/p` and press Tab.
- If multiple matches share a long common prefix, you may need to type beyond that prefix (e.g. add another letter) before pressing Tab to get a unique completion.
- Once the shell expands the full name (or you selected it via cycling), press Enter to run the command and read the flag.

#### Commands run: 

```sh
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwn (`/challenge/files/p<TAB>`)
pwn                    pwn-the-planet         pwncollege-flag        pwncollege-flyswatter  
pwn-college            pwncollege-family      pwncollege-flamingo    pwncollege-hacking     
hacker@globbing~multiple-options-for-tab-completion:~$ cat /challenge/files/pwncollege-flag
```

## Flag: 

```
pwn.college{Mo0JJlfzhg6H32ZbxCqZTFVtP8m.0lN0EzNxwyNwEzNzEzW}
```

### Notes:

- Single Tab in bash: completes to the longest common prefix; a second Tab lists possible completions.
- Double Tab (bash): prints all matches so you can choose a unique continuation.

# Challenge 10 Tab completion on commands

There is a command on the system whose name begins with `pwncollege`. You cannot (or should not) type the whole name. Start typing `pwncollege` and press the Tab key to let the shell complete the command name for you, then run it to receive the flag. This exercise demonstrates interactive command completion and reminds you to verify what the shell completed before executing.

## Solution:

- Shell completion works for commands as well as filenames. The shell looks through your PATH, aliases, functions, and builtin completions to find matching command names.
- Begin typing the command prefix `pwncollege` and press Tab. If there is exactly one matching command, the shell will complete the full command name.
- Important safety step: before pressing Enter to run a completed command, double-check what was expanded. A command completion could match an unexpected program, alias, or function.
- Once the command name is completed, press Enter to run it and read the flag.

#### Commands run: 

```sh
hacker@globbing~tab-completion-on-commands:~$ pwncollege-7245 (`pwncollege<TAB>`)
.bash_history  .college       .lesshst       catflag        not-the-flag   t              
.cache/        .config/       .local/        flag           pwn            temp           
hacker@globbing~tab-completion-on-commands:~$ pwncollege-7245 catflag (`pwncollege-7245 cat<TAB>`)
Correct! Here is your flag:
```

## Flag: 

```
pwn.college{cn1ljAC1pGoJ49ou8HXIQwmJJyk.0VN0EzNxwyNwEzNzEzW}
```

### Notes:

- Completion sources: the shell completes against builtins, aliases, functions, and executables in your PATH. `type -a <name>` shows which of those will be used.
- Bash behavior: single Tab completes to the longest common prefix; double Tab lists possibilities. Zsh may cycle or show a menu depending on config.
- Dangerous habit: never blindly Tab then Enter for commands you don’t recognize. Always verify the completed command name first.