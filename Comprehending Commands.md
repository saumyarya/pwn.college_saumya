# Challenge 1 cat: not the pet, but the command!

This level teaches the `cat` command, a fundamental tool used to read files (and concatenate multiple files). The challenge will copy the flag into a file inside your home directory; your job is to read that file using `cat`.

```sh
hacker@commands~cat-not-the-pet-but-the-command:~$ cat flag
```

## Solution:

- `cat` prints file contents to the terminal. It's perfect for quickly reading small files like flags.
- The challenge places the flag in a file inside your home directory (your shell starts in your home; `~` expands to `/home/hacker`).
- I used `cat` to read the file and captured the flag output.

## Flag: 

```
pwn.college{EDY-1RgLpqOzI3T4W_sDFzxvTU-.QXxcTN0wyNwEzNzEzW}
```

### Notes:

- Used `cat flag` to read the flag file and copied the output.

# Challenge 2 catting absolute paths

This level demonstrates reading a file via an **absolute path**. The file `/flag` is readable for this challenge; reading it with `cat /flag` reveals the flag.

```sh
hacker@commands~catting-absolute-paths:~$ cat /flag
```

## Solution:

- The prompt says `/flag` is readable from the sandbox; this is an absolute path pointing to the root filesystem.
- Use `cat` with the absolute path to print the file contents to the terminal.
- Copy the printed flag exactly (case sensitive) and paste it into this report.

## Flag: 

```
pwn.college{cU1MpFnMKYjvvbPHsrTYlnV6uJq.QX5ETO0wyNwEzNzEzW}
```

### Notes:

- ```/flag``` is an absolute path starting from root; using ```cat /flag``` reads it directly.
- Absolute paths ignore the current working directory, you always refer from ```/```.

# Challenge 3 more catting practice

In this level the sandbox prevents changing directories with `cd`. The flag is placed somewhere in the filesystem in a “crazy” directory. Since you cannot change your current working directory, you must reference the flag using an **absolute path** (a path that starts with `/`) and read it directly with `cat`.

```sh
hacker@commands~more-catting-practice:~$ cd /usr/lib/emacs/flag
You used 'cd'! In this level, I don't allow you to change the working directory 
--- you MUST chase pass 'cat' the absolute path of where I put it on the 
filesystem (which is /usr/lib/emacs/flag).
hacker@commands~more-catting-practice:~$ cat /usr/lib/emacs/flag
```

## Solution:

- The challenge restricts `cd`, so relative navigation is not allowed. Any solution must use an **absolute path** to access the flag file.
- The challenge gives you the path, use that directly with `cat /usr/lib/emacs`.
- Once you have the absolute path to the flag file, run `cat/usr/lib/emacs/flag` and copy the output.

## Flag: 

```
pwn.college{UKIiFjTagpkgcy3dPHlKcCQFofj.QXwITO0wyNwEzNzEzW}
```

### Notes:

- If cd is disabled, you must use absolute paths like /a/b/c/flagfile
- Mistake to avoid: assuming a file named flag lives in ```home``` or ````/challenge```, it might be elsewhere.

# Challenge 4 grepping for a needle in a haystack

This level teaches how to use `grep` to search inside very large files. The challenge provides a big file at `/challenge/data.txt` with 100,000 lines. We use `grep` to find the line(s) containing the flag.

```sh
hacker@commands~grepping-for-a-needle-in-a-haystack:~$ grep pwn.college /challenge/data.txt
```

## Solution:

- The file `/challenge/data.txt` is huge, so using `cat` and reading manually is impractical.
- The flag format always starts with `pwn.college`, so searching for that string will find the flag line.
- Use `grep` to search for `pwn.college` and print the matching line(s).
- For speed and neatness, extract only the first match and only the matching portion (if supported).

## Flag: 

```
pwn.college{YQWFZ12e9ogW7IqsGsB4Vyq-snd.QX3EDO0wyNwEzNzEzW}
```

### Notes:

- ```grep``` is ideal for searching huge files because it scans fast and can limit output

# Challenge 5 comparing files

This level has two files:  
- `/challenge/decoys_only.txt` — contains 100 fake flags  
- `/challenge/decoys_and_real.txt` — contains the same 100 fake flags **plus** the one real flag

Our goal is to compare the two files and extract the single line that appears only in the second file, that line is the real flag.

```sh
hacker@commands~comparing-files:~$ diff /challenge/decoys_only.txt /challenge/decoys_and_real.txt
24a25
> pwn.college{IEDFv04v7XFHGIOcx-hyVPnikFG.01MwMDOxwyNwEzNzEzW}
```

## Solution:

- `diff` compares two files line-by-line and shows the differences. Since `decoys_and_real.txt` has one extra (real) flag, `diff` will show that extra line with a `>` marker.
- Steps: run `diff` to reveal the differing line, copy the flag exactly.

## Flag: 

```
pwn.college{IEDFv04v7XFHGIOcx-hyVPnikFG.01MwMDOxwyNwEzNzEzW}
```

### Notes:

- ```diff``` shows differences; added lines in file2 appear prefixed by ```>```

# Challenge 6 listing files

This level hides the challenge program in `/challenge` under a random filename. Your job is to list the directory to discover the filename, then execute that program to retrieve the flag.

```sh
hacker@commands~listing-files:~$ ls /challenge
29918-renamed-run-21165  DESCRIPTION.md
hacker@commands~listing-files:~$ /challenge/29918-renamed-run-21165
Yahaha, you found me! Here is your flag:
```

## Solution:

- Since we don't know the program's name, list the `/challenge` directory to see what's inside.
- Once the filename is discovered (it will be an executable), run it by absolute path `/challenge/<name>`.
- Copy the printed flag exactly.

## Flag: 

```
pwn.college{Ip-zVLIEPy3pZU9dUHQPNiUKG6q.QX4IDO0wyNwEzNzEzW}
```

### Notes:

- The real challenge file was a renamed executable: ```29918-renamed-run-21165```
- ```DESCRIPTION.md``` is not executable — it’s just the human-readable guide.

# Challenge 7 touching files

This level teaches how to create files quickly with `touch`. The challenge asks you to create two files `/tmp/pwn` and `/tmp/college` and then run `/challenge/run` which will check those files and output the flag.

```sh
hacker@commands~touching-files:~$ touch /tmp/pwn
hacker@commands~touching-files:~$ touch /tmp/college
hacker@commands~touching-files:~$ /challenge/run
Success! Here is your flag:
```

## Solution:

- The prompt instructs us to create two empty files inside `/tmp`.
- `touch` is a simple command used to create empty files or update timestamps.
- After creating the files, execute `/challenge/run`. The program will read or verify the files and print the flag to stdout.

## Flag: 

```
pwn.college{w_37ko6LjR75okA4XKN6sqeZrqF.QXwMDO0wyNwEzNzEzW}
```

### Notes:

- ```touch filename``` creates an empty file or updates its timestamp if it already exists.

# Challenge 8 removing files

This level teaches how to create files quickly with `touch`. The challenge asks you to create two files `/tmp/pwn` and `/tmp/college` and then run `/challenge/run` which will check those files and output the flag.

```sh
hacker@commands~touching-files:~$ touch /tmp/pwn
hacker@commands~touching-files:~$ touch /tmp/college
hacker@commands~touching-files:~$ /challenge/run
Success! Here is your flag:
```

## Solution:

- The prompt instructs us to create two empty files inside `/tmp`.
- `touch` is a simple command used to create empty files or update timestamps.
- After creating the files, execute `/challenge/run`. The program will read or verify the files and print the flag to stdout.

## Flag: 

```
pwn.college{w_37ko6LjR75okA4XKN6sqeZrqF.QXwMDO0wyNwEzNzEzW}
```

### Notes:

- ```touch filename``` creates an empty file or updates its timestamp if it already exists.

# Challenge 9 moving files

This level teaches how to move files with `mv`. The goal is to move the system flag file `/flag` into the directory `/tmp/hack-the-planet`. After moving it, you run `/challenge/check` which verifies the move and prints the flag.

```sh
hacker@commands~moving-files:~$ mv /flag /tmp/hack-the-planet
Correct! Performing 'mv /flag /tmp/hack-the-planet'.
hacker@commands~moving-files:~$ /challenge/check
Congrats! You successfully moved the flag to /tmp/hack-the-planet! Here it is:
```

## Solution:

- The `mv` command renames or moves files. Moving `/flag` to `/tmp/hack-the-planet/` places it where the challenge checker expects it.
- Move the file with `mv /flag /tmp/hack-the-planet/`.
- Run `/challenge/check` to let the challenge verify the move and output the flag.

## Flag: 

```
pwn.college{oCBgd0bRWTB8EkKNHsAgyzc6Nit.0VOxEzNxwyNwEzNzEzW}
```

### Notes:

- ```mv``` moves or renames files; ```cp``` copies then ```rm``` can be used as a fallback.

# Challenge 10 hidden files

This level teaches about **hidden files** in Linux. Files whose names begin with `.` are hidden by default from `ls` and many other listings. You must search the root (`/`) for a dot-prefixed file that contains the flag and read it.


```sh
hacker@commands~hidden-files:~$ cd /
hacker@commands~hidden-files:/$ ls -a
.           .flag-2256190811268  challenge  home   lib64   mnt  proc  sbin  tmp
..          bin                  dev        lib    libx32  nix  root  srv   usr
.dockerenv  boot                 etc        lib32  media   opt  run   sys   var
hacker@commands~hidden-files:/$ cat .flag-2256190811268
```

## Solution:

- Linux hides files beginning with `.`. `ls` won't show them unless you ask for `-a` (all).
- The flag is stored in a dotfile somewhere under `/`, so we need to list or search for hidden files.
- List `/` with `ls -a` and inspect dotfiles.
- Once the flag file path is known, read it with `cat /path/to/.hiddenfile` and copy the printed flag.

## Flag: 

```
pwn.college{AzpgeHv43WMfUoOG_NPy_UkEfBH.QXxMDO0wyNwEzNzEzW}
```

### Notes:

- After locating the dotfile, cat prints its contents, copy the flag exactly (case-sensitive).

# Challenge 11 An Epic Filesystem Quest

This level is a little scavenger hunt. The sandbox hides a chain of clues (files named HINT / CLUE / similar) around the filesystem. Use `ls` to find each clue file, `cat` to read it, then follow its instructions (go to the next directory) until you reach and read the flag.

```sh
hacker@commands~an-epic-filesystem-quest:~$ cd /
hacker@commands~an-epic-filesystem-quest:/$ ls
GIST  challenge  flag  lib32   media  opt   run   sys  var
bin   dev        home  lib64   mnt    proc  sbin  tmp
boot  etc        lib   libx32  nix    root  srv   usr
hacker@commands~an-epic-filesystem-quest:/$ cat GIST
Tubular find!
The next clue is in: /opt/linux/linux-5.4/include/linux/iio/imu

hacker@commands~an-epic-filesystem-quest:/$ cd /opt/linux/linux-5.4/include/linux/iio/imu
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/include/linux/iio/imu$ ls
MEMO  adis.h
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/include/linux/iio/imu$ cat MEMO
Great sleuthing!
The next clue is in: /usr/share/doc/libpam-runtime

The next clue is **hidden** --- its filename starts with a '.' character. You'll need to look for it using special options to 'ls'.

hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/include/linux/iio/imu$ cd /usr/share/doc/libpam-runtime
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/libpam-runtime$ ls -a
.  ..  .MESSAGE  NEWS.Debian.gz  changelog.Debian.gz  copyright
hacker@commands~an-epic-filesystem-quest:/usr/share/doc/libpam-runtime$ cat .MESSAGE
Congratulations, you found the clue!
The next clue is in: /usr/share/racket/pkgs/racket-doc/scribblings/guide/contracts/examples

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.

hacker@commands~an-epic-filesystem-quest:/usr/share/doc/libpam-runtime$ cd /usr/share/racket/pkgs/racket-doc/scribblings/guide/contracts/examples
hacker@commands~an-epic-filesystem-quest:/usr/share/racket/pkgs/racket-doc/scribblings/guide/contracts/examples$ ls
1-test.rkt  2.rkt       5.rkt            ho-version2.rkt   ho-version3b.rkt
1.rkt       3-test.rkt  NUGGET           ho-version2a.rkt  ho-version4.rkt
1b.rkt      3.rkt       compiled         ho-version3.rkt
2-test.rkt  5-test.rkt  ho-version1.rkt  ho-version3a.rkt
hacker@commands~an-epic-filesystem-quest:/usr/share/racket/pkgs/racket-doc/scribblings/guide/contracts/examples$ cat NUGGET
Yahaha, you found me!
The next clue is in: /usr/local/lib/python3.8/dist-packages/future/backports/test/__pycache__

hacker@commands~an-epic-filesystem-quest:/usr/share/racket/pkgs/racket-doc/scribblings/guide/contracts/examples$ cd /usr/local/lib/python3.8/dist-packages/future/backports/test/__pycache__
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/future/backports/test/__pycache__$ ls
INSIGHT                  pystone.cpython-38.pyc      support.cpython-38.pyc
__init__.cpython-38.pyc  ssl_servers.cpython-38.pyc
hacker@commands~an-epic-filesystem-quest:/usr/local/lib/python3.8/dist-packages/future/backports/test/__pycache__$ cat INSIGHT
Yahaha, you found me!
The next clue is in: /usr/lib/python3/dist-packages/pluggy/__pycache__

Watch out! The next clue is **trapped**. You'll need to read it out without 'cd'ing into the directory; otherwise, the clue will self destruct!

hacker@commands~an-epic-filesystem-quest:~$ ls /usr/lib/python3/dist-packages/pluggy/__pycache__
SECRET-TRAPPED           _tracing.cpython-38.pyc  callers.cpython-38.pyc  manager.cpython-38.pyc
__init__.cpython-38.pyc  _version.cpython-38.pyc  hooks.cpython-38.pyc
hacker@commands~an-epic-filesystem-quest:~$ cat /usr/lib/python3/dist-packages/pluggy/__pycache__/SECRET-TRAPPED
Tubular find!
The next clue is in: /opt/linux/linux-5.4/include/config/usb/announce

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.

hacker@commands~an-epic-filesystem-quest:~$ cd /opt/linux/linux-5.4/include/config/usb/announce
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/include/config/usb/announce$ ls
HINT  new
hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/include/config/usb/announce$ cat HINT
Yahaha, you found me!
The next clue is in: /usr/share/javascript/mathjax/unpacked/jax/output/SVG/fonts/STIX-Web/Size3/Regular

hacker@commands~an-epic-filesystem-quest:/opt/linux/linux-5.4/include/config/usb/announce$ cd /usr/share/javascript/mathjax/unpacked/jax/output/SVG/fonts/STIX-Web/Size3/Regular
hacker@commands~an-epic-filesystem-quest:/usr/share/javascript/mathjax/unpacked/jax/output/SVG/fonts/STIX-Web/Size3/Regular$ ls
DOSSIER  Main.js
hacker@commands~an-epic-filesystem-quest:/usr/share/javascript/mathjax/unpacked/jax/output/SVG/fonts/STIX-Web/Size3/Regular$ cat DOSSIER
Congratulations, you found the clue!
The next clue is in: /usr/share/javascript/mathjax/jax/output/SVG/fonts/Latin-Modern/Main/Regular

The next clue is **delayed** --- it will not become readable until you enter the directory with 'cd'.

hacker@commands~an-epic-filesystem-quest:/usr/share/javascript/mathjax/unpacked/jax/output/SVG/fonts/STIX-Web/Size3/Regular$ cd /usr/share/javascript/mathjax/jax/output/SVG/fonts/Latin-Modern/Main/Regular
hacker@commands~an-epic-filesystem-quest:/usr/share/javascript/mathjax/jax/output/SVG/fonts/Latin-Modern/Main/Regular$ ls
BRIEF  Main.js
hacker@commands~an-epic-filesystem-quest:/usr/share/javascript/mathjax/jax/output/SVG/fonts/Latin-Modern/Main/Regular$ cat BRIEF
CONGRATULATIONS! Your perserverence has paid off, and you have found the flag!

```

## Solution:

- Start at `/` as directed, list files to find the first clue (filename like `HINT` or `CLUE`).
- `cat` the clue file to discover the next location or instruction.
- Move to the indicated directory and repeat: `ls`, `cat`, follow instruction.
- Continue following breadcrumbs until you locate and `cat` the final file that contains the `pwn.college{...}` flag.
- Copy the flag exactly (case-sensitive) and paste it into this report.

## Flag: 

```
pwn.college{Udiw0ANNnKLM-BW0GhjWK1glbQg.QX5IDO0wyNwEzNzEzW}
```

### Notes:

- Use ```ls -a``` to reveal hidden/permission info and spot files named HINT/CLUE.
- ```cat``` prints clue contents- follow them exactly.
- We can list items in a directory without `cding` into the directory

# Challenge 12 making directories

This level teaches how to create directories with `mkdir` and files with `touch`. The challenge asks you to create a directory `/tmp/pwn`, create a file named `college` inside it, and then run `/challenge/run`, which will check for those files and print the flag.


```sh
hacker@commands~making-directories:~$ mkdir /tmp/pwn
hacker@commands~making-directories:~$ cd /tmp/pwn
hacker@commands~making-directories:/tmp/pwn$ touch college
hacker@commands~making-directories:/tmp/pwn$ ls
college
hacker@commands~making-directories:/tmp/pwn$ /challenge/run
Success! Here is your flag:
```

## Solution:

- The `/tmp` directory is writable and ideal for temporary files and testing.
- Use `mkdir -p /tmp/pwn` to create the directory (the `-p` prevents errors if it already exists).
- Use `touch /tmp/pwn/college` to create the required file inside that directory.
- Verify the directory and file exist with `ls /tmp/pwn`.
- Run `/challenge/run` which will inspect `/tmp/pwn` and output the flag.
- Copy the flag exactly as printed.

## Flag: 

```
pwn.college{kgqanoMjwIo_BmAQo1xhTDlo5lc.QXwUDO0wyNwEzNzEzW}
```

### Notes:

- `mkdir -p` safely creates parent dirs if needed.
- `touch` creates empty files or updates timestamps.
- `/tmp` is a writable temporary directory good for these exercises.

# Challenge 13 finding files

This level requires using the `find` command to search the filesystem for a file named `flag`. Because there may be many `flag` files and some directories are inaccessible to the normal user, we use safe search patterns and filters to locate the real flag efficiently.


```sh
hacker@commands~finding-files:~$ find / -name flag
find: ‘/tmp/tmp.4mK6TfTSUV’: Permission denied
find: ‘/etc/ssl/private’: Permission denied
/usr/local/lib/python3.8/dist-packages/pwnlib/flag
/usr/share/javascript/mathjax/jax/output/HTML-CSS/fonts/Gyre-Pagella/Size4/Regular/flag
find: ‘/var/cache/apt/archives/partial’: Permission denied
find: ‘/var/cache/ldconfig’: Permission denied
find: ‘/var/cache/private’: Permission denied
find: ‘/var/lib/apt/lists/partial’: Permission denied
find: ‘/var/lib/mysql-files’: Permission denied
find: ‘/var/lib/private’: Permission denied
find: ‘/var/lib/mysql’: Permission denied
find: ‘/var/lib/mysql-keyring’: Permission denied
find: ‘/var/lib/php/sessions’: Permission denied
find: ‘/var/log/private’: Permission denied
find: ‘/var/log/apache2’: Permission denied
find: ‘/var/log/mysql’: Permission denied
find: ‘/run/mysqld’: Permission denied
find: ‘/run/sudo’: Permission denied
find: ‘/root’: Permission denied
/opt/pwndbg/.venv/lib/python3.8/site-packages/pwnlib/flag
find: ‘/proc/tty/driver’: Permission denied
find: ‘/proc/1/task/1/fd’: Permission denied
find: ‘/proc/1/task/1/fdinfo’: Permission denied
find: ‘/proc/1/task/1/ns’: Permission denied
find: ‘/proc/1/fd’: Permission denied
find: ‘/proc/1/map_files’: Permission denied
find: ‘/proc/1/fdinfo’: Permission denied
find: ‘/proc/1/ns’: Permission denied
find: ‘/proc/7/task/7/fd’: Permission denied
find: ‘/proc/7/task/7/fdinfo’: Permission denied
find: ‘/proc/7/task/7/ns’: Permission denied
find: ‘/proc/7/fd’: Permission denied
find: ‘/proc/7/map_files’: Permission denied
find: ‘/proc/7/fdinfo’: Permission denied
find: ‘/proc/7/ns’: Permission denied
/nix/store/ka6xbd6z6wj5d6frl7ym4nzfc6p2wkdx-radare2-5.9.8/share/radare2/5.9.8/flag
/nix/store/f31j0igg7ms3yrj5gm3cm76bjcmdl8w5-python3.12-pwntools-4.13.1/lib/python3.12/site-packages/pwnlib/flag
/nix/store/7ns27apnvn4qj4q5c82x0z1lzixrz47p-radare2-5.9.8/share/radare2/5.9.8/flag
/nix/store/5z3sjp9r463i3siif58hq5wj5jmy5m98-python3.12-pwntools-4.13.1/lib/python3.12/site-packages/pwnlib/flag
/nix/store/n6vb30rd7kkwjj595pgm0dmsmfaqi6i5-rizin-0.7.3/share/rizin/flag
/nix/store/5n5lp1m8gilgrsriv1f2z0jdjk50ypcn-rizin-0.7.3/share/rizin/flag
/nix/store/bnlabj2vsbljhp597ir29l51nrqhm89w-rizin-0.7.4/share/rizin/flag
/nix/store/s8b49lb0pqwvw0c6kgjbxdwxcv2bp0x4-radare2-5.9.8/share/radare2/5.9.8/flag
/nix/store/8qvj9mzdq2qxgjigw4ysqgbkcx1zi80y-python3.13-pwntools-4.14.1/lib/python3.13/site-packages/pwnlib/flag
/nix/store/1hyxipvwpdpcxw90l5pq1nvd6s6jdi5m-python3.12-pwntools-4.14.1/lib/python3.12/site-packages/pwnlib/flag
/nix/store/dz2qxywk6d8hc1gkarpwbhyxb50sh2ak-pwntools-4.14.0/lib/python3.13/site-packages/pwnlib/flag
hacker@commands~finding-files:~$ cat /usr/local/lib/python3.8/dist-packages/pwnlib/flag
cat: /usr/local/lib/python3.8/dist-packages/pwnlib/flag: Is a directory
hacker@commands~finding-files:~$ cat /usr/share/javascript/mathjax/jax/output/HTML-CSS/fonts/Gyre-Pagella/Size4/Regular/flag
```

## Solution:

- `find` walks the filesystem and matches files by criteria like name, type, or modification time.
- The challenge says the file is named `flag` but may be hidden in a random directory. There are other `flag` files too, so we must read any candidate file to verify it contains the real flag string `pwn.college{...}`.
- Use `find` starting from root `/`.
- Use `-name` to match the filename.
- Once the correct file is found, `cat` it and copy the printed flag.

## Flag: 

```
pwn.college{AvjdDIsOj5nFyP3feUCq5jXdTp5.QXyMDO0wyNwEzNzEzW}
```

### Notes:

- ```find / -name flag``` lists candidate files named flag. Many will be inaccessible, ignore ```Permission denied``` errors.

# Challenge 14 linking files

In this level, the challenge program `/challenge/catflag` reads `/home/hacker/not-the-flag`. We create a **symbolic link** at `~/not-the-flag` that points to `/flag`, tricking the program into reading the real flag.


```sh
hacker@commands~linking-files:~$ ln -s /flag ~/not-the-flag
hacker@commands~linking-files:~$ file ~/not-the-flag
/home/hacker/not-the-flag: symbolic link to /flag
hacker@commands~linking-files:~$ /challenge/catflag
About to read out the /home/hacker/not-the-flag file!
```

## Solution:

- The program expects to read `/home/hacker/not-the-flag`.
- By creating a symbolic link `~/not-the-flag` -> `/flag`, we redirect reads of the not-the-flag path to the real `/flag`.
- This uses `ln -s /flag ~/not-the-flag` (target first, link name second).

## Flag: 

```
pwn.college{0rkUdq01M4kk_k_Wtov0CUflihW.QX5ETN1wyNwEzNzEzW}
```

### Notes:

- ln -s <target> <link> creates a symlink; accessing the link follows to the target.
- Symlinks are super useful for redirecting file access without moving files.
