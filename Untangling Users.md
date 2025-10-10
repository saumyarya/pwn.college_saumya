# Challenge 1 Becoming root with su

You must use the `su` utility to become `root` (this challenge has a root password). The root password is hack-the-planet. Once you are root, read the flag file.

## Solution:

- `su` is a setuid root binary so running it will attempt to start a root shell after authenticating the root password.
- Run `su` and enter the known root password hack-the-planet. If authentication succeeds, your shell becomes `root` (prompt usually changes to `#`).
- As root, locate and print the flag.

#### Commands run: 

```sh
hacker@users~becoming-root-with-su:~$ su
Password: 
root@users~becoming-root-with-su:/home/hacker# cat /flag
pwn.college{0Du5oljfyZptPZxzvaHeX1KMGyJ.QX1UDN1wyNwEzNzEzW}
root@users~becoming-root-with-su:/home/hacker# exit
exit
```

## Flag: 

```
pwn.college{0Du5oljfyZptPZxzvaHeX1KMGyJ.QX1UDN1wyNwEzNzEzW}
```

### Notes:

- If `su` says Authentication failure, ensure exact password hack-the-planet (no quotes), correct keyboard layout, and that Caps Lock is off.

# Challenge 2 Other users with su

You must switch user to zardus using `su` (zardus' password is `dont-hack-me`) and then run `/challenge/run`. The program will print the flag.

## Solution:

- `su <user>` will prompt for that user’s password and, on success, give you a shell as that user.
- Use the provided password `dont-hack-me` to authenticate as zardus.
- Once you are the zardus user, execute `/challenge/run` from that shell. Many challenge binaries print the flag directly.

#### Commands run: 

```sh
hacker@users~other-users-with-su:~$ su zardus
Password: 
zardus@users~other-users-with-su:/home/hacker$ /challenge/run
Congratulations, you have become Zardus! Here is your flag:
pwn.college{wZH1jDZFQdLnu4bwLcQEHBa80ns.QX2UDN1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{wZH1jDZFQdLnu4bwLcQEHBa80ns.QX2UDN1wyNwEzNzEzW}
```

### Notes:

- Password mistakes: ensure exact typing: dont-hack-me (no trailing spaces, Caps Lock off).

# Challenge 3 Cracking passwords

A leaked `/etc/shadow` is available at `/challenge/shadow-leak`. Crack the hash to recover zardus’ password, `su` to the zardus account, and run `/challenge/run` to get the flag.

## Solution:

- Inspect the leaked file to confirm which hashes it contains.
- Use John the Ripper to crack the hash. John will auto-detect common crypt formats.
- `su zardus` and enter the cracked password. `/challenge/run` will print the flag.

#### Commands run: 

```sh
hacker@users~cracking-passwords:~$ john /challenge/shadow-leak
Created directory: /home/hacker/.john
Loaded 1 password hash (crypt, generic crypt(3) [?/64])
Press 'q' or Ctrl-C to abort, almost any other key for status
aardvark         (zardus)
1g 0:00:00:20 100% 2/3 0.04970g/s 289.4p/s 289.4c/s 289.4C/s Johnson..buzz
Use the "--show" option to display all of the cracked passwords reliably
Session completed
hacker@users~cracking-passwords:~$ su zardus
Password: 
zardus@users~cracking-passwords:/home/hacker$ /challenge/run
Congratulations, you have become Zardus! Here is your flag:
pwn.college{APt_yOUEbDvt5cYCIx8VIgmnaDW.QX3UDN1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{APt_yOUEbDvt5cYCIx8VIgmnaDW.QX3UDN1wyNwEzNzEzW}
```

### Notes:

- Modern Linux shadow hashes beginning with `$6$` are `sha512crypt`. John typically recognizes and cracks them with `john <file>`. If John fails to auto-detect, use `--format=sha512crypt`.
Example: `john --format=sha512crypt /challenge/shadow-leak`

# Challenge 4 Using sudo

You are given a user account that has `sudo` privileges for some command(s). Use the `sudo` policy to run a command as root and read the flag.

## Solution:

- Confirm our identity and environment: `whoami` and list files to find the flag path.
- Check which commands we are allowed to run as root with `sudo -l`. This reveals the `sudo` policy for our user (which commands can be run and whether a password is needed).
- If `sudo -l` shows we can run a shell-reading command (e.g., `cat`, `less`, `grep`, `tee`, etc.) as root on arbitrary files or on the flag, run that command with `sudo` to print the flag.

#### Commands run: 

```sh
hacker@users~using-sudo:~$ sudo cat /flag
pwn.college{oaZ0rQkXUxjrdjk4bPcb42hnl4X.QX4UDN1wyNwEzNzEzW}
```

## Flag: 

```
pwn.college{oaZ0rQkXUxjrdjk4bPcb42hnl4X.QX4UDN1wyNwEzNzEzW}
```

### Notes:

- `sudo -l' is your first, best friend, always run it to see allowed commands before guessing.