# Challenge 1 Changing File Ownership

The `/flag` file is owned by `root` and not readable by your user. For this level you (the hacker user) are allowed to run `chown` on files. Change the owner of `/flag` to yourself and read the flag.

## Solution:

- Confirm identity and the flag's current ownership with `whoami` and `ls -l /flag`.
- Use `chown` to change the owner of `/flag` to your user (e.g., `chown hacker /flag`).
- Verify ownership changed and then read the flag with `cat /flag`.

#### Commands run: 

```sh
hacker@permissions~changing-file-ownership:~$ chown hacker /flag
hacker@permissions~changing-file-ownership:~$ cat /flag
pwn.college{AMRITRarn-6S-MVN7WIHnzAqnzy.QXxEjN0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{AMRITRarn-6S-MVN7WIHnzAqnzy.QXxEjN0wyNwEzNzEzW}
```

### Notes:

- After `chown`, a quick `ls -l /flag` confirms ownership before `cat`.

# Challenge 2 Groups and Files

The `/flag` file is owned by `root:root` and only readable by its group. You (the `hacker` user) are allowed to run `chgrp`. Change the group owner of `/flag` to a group you belong to (for example `hacker`) so you can read it.

## Solution:

- Confirm who you are and what groups you belong to with id.
- Inspect `/flag` to see current `owner/group` and permissions with `ls -l /flag`. Note whether the group has read permission (e.g., `-r--r-----` means group can read).
- Change the group owner to one of your groups (commonly `hacker`) using `chgrp hacker /flag`. The level allows `chgrp` as the `hacker` user, so `sudo` is not required here.
- Verify the change with `ls -l /flag` and then read the file with `cat /flag`.

#### Commands run: 

```sh
hacker@permissions~groups-and-files:~$ chgrp hacker /flag
hacker@permissions~groups-and-files:~$ cat /flag
pwn.college{oh8vzJEyBwgkziYfoH-jS5HpPnW.QXxcjM1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{oh8vzJEyBwgkziYfoH-jS5HpPnW.QXxcjM1wyNwEzNzEzW}
```

### Notes:

- If `id` shows you are not in the `hacker` group, pick a group you are in (from `id`) when running `chgrp`

# Challenge 3 Fun With Groups Names

Your account belongs to a randomized group name in this level. Use `id` to discover the `group` your user is in, then `chgrp` the `/flag` file to that `group` so you can read it.

## Solution:

- Run `id` to see your primary group name (the `gid(name)` part).
- Confirm `/flag` is group-readable with `ls -l /flag` (the group permission bit must include `r`).
- Use `chgrp <your-group>` to change the group owner of `/flag` to the group you found in step 1.
- Verify ownership changed and `cat /flag` to read the flag.

#### Commands run: 

```sh
hacker@permissions~fun-with-groups-names:~$ id
uid=1000(hacker) gid=1000(grp1070) groups=1000(grp1070)
hacker@permissions~fun-with-groups-names:~$ chgrp grp1070 /flag
hacker@permissions~fun-with-groups-names:~$ cat /flag
pwn.college{Mch4dhcP1_gNARXCIJ7rDqorOlZ.QXycjM1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{Mch4dhcP1_gNARXCIJ7rDqorOlZ.QXycjM1wyNwEzNzEzW}
```

### Notes:

- The group name you need is the one shown after `gid=` in the id output (e.g. `gid=1000(randomname)`). Use that exact name in `chgrp`.
- If `ls -l /flag` shows group lacks the `r` bit (e.g., `-rw-------`), changing group alone won't help, this level is set so the group has read permission.

# Challenge 4 Changing Permissions

The `/flag` file is owned by `root` and originally only readable by its owner (e.g. `-r--------`). In this level `chmod` is allowed to change permissions even though you are the hacker user. Your goal: change `/flag`’s permissions so you can read it and print the flag.

## Solution:

- Inspect the file to confirm current ownership and permissions (`ls -l /flag`).
- Decide which permission class needs read access. Because you are not the owning user or in the owning group, adding others read (`o+r`) will make the file readable by you. Alternatively `a+r` gives read to user,group,others.
- Use `chmod` to add read permission for others (or all) on `/flag`. This modifies the mode without needing ownership thanks to the level’s rules.
- `cat /flag` to read the flag.

#### Commands run: 

```sh
hacker@permissions~changing-permissions:~$ chmod o+r /flag
hacker@permissions~changing-permissions:~$ cat /flag
pwn.college{cJlsCF-oxD6HhMVyQNHDJIwhr28.QXzcjM1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{cJlsCF-oxD6HhMVyQNHDJIwhr28.QXzcjM1wyNwEzNzEzW}
```

### Notes:

- Using `o+r` is the least intrusive: it only grants read to “others.” `a+r` also works but is broader.

# Challenge 5 Executable Files

`/challenge/run` is owned by `root` and currently lacks an execute bit for you. The binary prints the flag when executed, but Linux will deny execution unless the appropriate execute permission is set. Your goal: change `/challenge/run`’s mode so the `hacker` user can execute it, then run it to get the flag.

## Solution:

- Confirm current mode and ownership with `ls -l /challenge/run`.
- Because the file is owned by `root` and the owning group is `root`, give the others/users class execute permission so non-owner users (like hacker) can run it. The least-invasive change is `u+x`. `a+x` also works (gives group execute too).
- Use `chmod o+x /challenge/run` to add the execute bit for others.
- Run `/challenge/run` to print the flag.

#### Commands run: 

```sh
hacker@permissions~executable-files:~$ chmod o+x /challenge/run
hacker@permissions~executable-files:~$ /challenge/run
bash: /challenge/run: Permission denied
hacker@permissions~executable-files:~$ chmod u+x /challenge/run
hacker@permissions~executable-files:~$ /challenge/run
Successful execution! Here is your flag:
pwn.college{AC9T_UoNlkykzAKWF3PapPLQjFv.QXyEjN0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{AC9T_UoNlkykzAKWF3PapPLQjFv.QXyEjN0wyNwEzNzEzW}
```

### Notes:

- adding `+x` without specifying class defaults to `a+x` for many shells, that's fine, but be explicit when you want minimal changes.

# Challenge 6 Permission Tweaking Practice

A sequence of eight rounds. Each round `/challenge/run` tells you the exact permission bits it wants on `/challenge/pwn`. Use `chmod` to set them exactly; a mistake resets the gauntlet. After 8 correct rounds you are allowed to `chmod /flag` and then read it.

## Solution:

- Run `/challenge/run` to get the target mode for the current round.
- Convert the requested mode into a `chmod` command, prefer symbolic for incremental tweaks (`u+x`, `g+w`, `go-rwx`, etc.) and numeric when the challenge expects a full overwrite.
- Apply `chmod` and immediately verify.
- Repeat until you complete 8 rounds. When granted permission to `chmod /flag`, add the read bit for the appropriate class (`user/group/other`) and `cat /flag`.

#### Commands run: 

```sh
hacker@permissions~permission-tweaking-practice:~$ /challenge/run
Round 1 of 8!
hacker@permissions~permission-tweaking-practice:~$ chmod go+x /challenge/pwn
You set the correct permissions!
Round 2 of 8!
hacker@permissions~permission-tweaking-practice:~$ chmod u-w /challenge/pwn 
You set the correct permissions!
Round 3 of 8!
hacker@permissions~permission-tweaking-practice:~$ chmod u+x /challenge/pwn
You set the correct permissions!
Round 4 of 8!
hacker@permissions~permission-tweaking-practice:~$ chmod u+w /challenge/pwn
You set the correct permissions!
Round 5 of 8!
hacker@permissions~permission-tweaking-practice:~$ chmod o+w /challenge/pwn
You set the correct permissions!
Round 6 of 8!
hacker@permissions~permission-tweaking-practice:~$ chmod g+w /challenge/pwn
You set the correct permissions!
Round 7 of 8!
hacker@permissions~permission-tweaking-practice:~$ chmod go-rwx /challenge/pwn
You set the correct permissions!
Round 8 of 8!
hacker@permissions~permission-tweaking-practice:~$ chmod o+wx /challenge/pwn
You set the correct permissions!
You've solved all 8 rounds! I have changed the ownership
of the /flag file so that you can 'chmod' it. You won't be able to read
it until you make it readable with chmod!
hacker@permissions~permission-tweaking-practice:~$ chmod u+r /flag
hacker@permissions~permission-tweaking-practice:~$ cat /flag
pwn.college{U43euTPEnzprnFKpda9yqCk3fvK.QXwEjN0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{U43euTPEnzprnFKpda9yqCk3fvK.QXwEjN0wyNwEzNzEzW}
```

### Notes:

- Use symbolic (`u/g/o + +/-/=`) for precise incremental changes, it avoids overwriting unrelated bits.

# Challenge 7 Permissions Setting Practice

This level expands the `chmod` toolbox: instead of only adding/removing bits, you must sometimes overwrite permission sets using `=` and chain different class assignments with commas. The challenge will ask you to set exact, possibly radical, modes on `/challenge/pwn` (for example giving the user rw, the group r, and wiping world). Use `u=`, `g=`, `o=`, comma-chaining, and `-` (as a full-zero for a class) where required. Verify after each change with `ls -l`. Get the requested modes until the level completes.

## Solution:

- Read the prompt from `/challenge/run` to see the exact required mode (it will expect a full specification for `user/group/world`).
- Prefer `=` when the challenge expects exact overwrites (it wipes whatever bits were there). Use `+/-` only for incremental tweaks.
- Chain different class assignments with commas to set different classes differently: `chmod u=rw,g=r,o=- /challenge/pwn` sets `user rw`, `group r`, `world nothing`.
- Then at the end set the permission for `/flag` and read it by `cat /flag`.

#### Commands run: 

```sh
hacker@permissions~permissions-setting-practice:~$ /challenge/run
Round 1 of 8!
hacker@permissions~permissions-setting-practice:~$ chmod u=r,o=rx /challenge/pwn
You set the correct permissions!
Round 2 of 8!
hacker@permissions~permissions-setting-practice:~$ chmod u=-,g=x,o=r /challenge/pwn
You set the correct permissions!
Round 3 of 8!
hacker@permissions~permissions-setting-practice:~$ chmod u=r,g=rw,o=x /challenge/pwn
You set the correct permissions!
Round 4 of 8!
hacker@permissions~permissions-setting-practice:~$ chmod u=w,g=w,o=rw /challenge/pwn
You set the correct permissions!
Round 5 of 8!
hacker@permissions~permissions-setting-practice:~$ chmod u=rwx,g=rw,o=r /challenge/pwn
You set the correct permissions!
Round 6 of 8!
hacker@permissions~permissions-setting-practice:~$ chmod u=x,g=w,o=- /challenge/pwn
You set the correct permissions!
Round 7 of 8!
hacker@permissions~permissions-setting-practice:~$ chmod u=wx,g=r,o=w /challenge/pwn
You set the correct permissions!
Round 8 of 8!
hacker@permissions~permissions-setting-practice:~$ chmod u=w,g=w,o=r /challenge/pwn
You set the correct permissions!
You've solved all 8 rounds! I have changed the ownership
of the /flag file so that you can 'chmod' it. You won't be able to read
it until you make it readable with chmod!
hacker@permissions~permissions-setting-practice:~$ chmod u=r /flag
hacker@permissions~permissions-setting-practice:~$ cat /flag
pwn.college{UH4eMpWujP3MGeHYcIR8XjlOmF0.QXzETO0wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{UH4eMpWujP3MGeHYcIR8XjlOmF0.QXzETO0wyNwEzNzEzW}
```

### Notes:

- Remember `o=-` (a literal `-` after `=`) means “no permissions for other”; it is not the same as `o-` which removes specific bits.

# Challenge 8 The SUID Bit

Give the `/challenge/getroot` binary the `SUID bit` so when you execute it, it runs as the file’s owner (`root`) and (in this level) spawns a `root` shell. Then use that shell to `cat /flag`.

## Solution:

- `u+s` sets the `Set-User-ID` on execute: when a user runs the program, the kernel runs it with the owner’s privileges (here: `root`) instead of the caller’s.
- The `s` appears in `ls -l` where the owner `x` would normally be (`-rwsr-xr-x`).
- Because `SUID` on root-owned binaries is powerful, only set it on a trusted program (the challenge intentionally allows this).

#### Commands run: 

```sh
hacker@permissions~the-suid-bit:~$ chmod u+s /challenge/getroot
hacker@permissions~the-suid-bit:~$ /challenge/getroot
SUCCESS! You have set the suid bit on this program, and it is running as root! 
Here is your shell...
root@permissions~the-suid-bit:~# cat /flag
pwn.college{cnzjkn4y32SbamrhbW3C1Mt_7qG.QXzEjN0wyNwEzNzEzW}
root@permissions~the-suid-bit:~# exit
exit
```

## Flag: 

```
pwn.college{cnzjkn4y32SbamrhbW3C1Mt_7qG.QXzEjN0wyNwEzNzEzW}
```

### Notes:

- If `ls -l` still shows no `s`, make sure you ran `chmod` with the correct path and that the file is owned by `root`.