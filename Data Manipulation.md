# Challenge 1 Translating characters

`/challenge/run` prints the flag but with every letter's case swapped (A-a, a-A). Use `tr` to undo the case swap and recover the real flag.

## Solution:

- Run `/challenge/run` to see the (case-swapped) output. It prints the flag but with letter cases inverted.
- We need a tool that reads stdin, translates characters, and writes to stdout, `tr` does exactly that. `tr` can map ranges A-Za-z to a-zA-Z to invert case.
- Pipe the output of `/challenge/run` into `tr` so it performs the character translation on the go.
- Verify the output looks like `pwn.college{...}`

#### Commands run: 

```sh
hacker@data~translating-characters:~$ /challenge/run
Your case-swapped flag:
PWN.COLLEGE{ORKzabmptJgSDI_3b7kWHSac-uJ.01mXeZnXWYnWeZnZeZw}
hacker@data~translating-characters:~$ /challenge/run | tr A-Za-z a-zA-Z
yOUR CASE-SWAPPED FLAG:
```

## Flag: 

```
pwn.college{orkZABMPTjGsdi_3B7KwhsAC-Uj.01MxEzNxwyNwEzNzEzW}
```

### Notes:

- `tr` reads stdin and writes stdout; use a pipe (`|`) to feed it output from other commands.

# Challenge 2 Deleting characters

`/challenge/run` prints the flag but with decoy characters `^` and `%` interspersed among the real characters. Use `tr -d` to delete those decoys and recover the real flag.

## Solution:

- Run `/challenge/run` to see the output (flag characters mixed with `^` and `%`).
- `tr -d` removes any characters listed in its argument from stdin.
- Pipe the program output into `tr -d` and list the decoy characters `^%` as the deletion set (quoted to avoid shell expansion).
- The resulting stdout will be the clean flag.

#### Commands run: 

```sh
hacker@data~deleting-characters:~$ /challenge/run
Your character-stuffed flag:
p^%w%n%.%c^o^l%l^%eg%e{U%ud%v^4%Nl^-%m%p^%I%B^8^%k^%S%cN^%_^%0t^%paTHf^N^%K^%.0^%F^N%xE^%z%N^x^%w^y%N^w^%E^%z^%N^%z^%E^%z%W^%}^%^%
hacker@data~deleting-characters:~$ /challenge/run | tr -d '^%'
Your character-stuffed flag:
pwn.college{Uudv4Nl-mpIB8kScN_0tpaTHfNK.0FNxEzNxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{Uudv4Nl-mpIB8kScN_0tpaTHfNK.0FNxEzNxwyNwEzNzEzW}
```

### Notes:

- order doesn't matter, tr -d '%^' works the same.

# Challenge 3 Deleting newlines

`/challenge/run` prints the flag but with extra newline characters injected between or inside the real characters. Use `tr -d '\n'` to remove those newlines and produce the flag on a single line.

## Solution:

- Run `/challenge/run` to see the flag output (with many newlines).
- `tr -d '\n'` deletes newline characters from stdin.
- Pipe the program output into `tr -d '\n'` to join everything into one line and reveal the flag.

#### Commands run: 

```sh
hacker@data~deleting-newlines:~$ /challenge/run
Your line-split flag: 
p
w
n.
c
ol
l
eg
e
{s
W
hu
R
j
j
k
B
j
HY
5
4Y7
l
S
9
V1BuIP
_
G
.0
V
Nx
E
z
Nxw
y
N
w
E
z
NzEz
W}


hacker@data~deleting-newlines:~$ /challenge/run | tr -d '\n'
Your line-split flag: pwn.college{sWhuRjjkBjHY54Y7lS9V1BuIP_G.0VNxEzNxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{sWhuRjjkBjHY54Y7lS9V1BuIP_G.0VNxEzNxwyNwEzNzEzW}
```

### Notes:

- Quote '\n' so the shell passes the backslash escape to `tr` rather than interpreting it.
- This removes newline characters only; it won't remove other whitespaces.

# Challenge 4 Extracting the first line with heads

`/challenge/pwn` prints a lot of output. Pipe its output to head to keep only the first 7 lines, then pipe those 7 lines into `/challenge/college` to get the flag.

## Solution:

- Run `/challenge/pwn` to produce the verbose output.
- Use `head -n 7` to keep only the first seven lines of that stream.
- Pipe the result into `/challenge/college`, which will check the (shortened) input and print the flag if it's correct.

#### Commands run: 

```sh
hacker@data~extracting-the-first-lines-with-head:~$ /challenge/pwn | head -n 7
SECRET-CODE-11579
SECRET-CODE-32157
SECRET-CODE-25880
SECRET-CODE-17315
SECRET-CODE-1878
SECRET-CODE-10072
SECRET-CODE-11287
hacker@data~extracting-the-first-lines-with-head:~$ /challenge/pwn | head -n 7 | /challenge/college
Congratulations, you piped the right codes!
pwn.college{4SFktL6VTMqpTp9-_HlvU1KQEhJ.0lNxEzNxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{4SFktL6VTMqpTp9-_HlvU1KQEhJ.0lNxEzNxwyNwEzNzEzW}
```

### Notes:

- `head -n 7` and `head -7` are equivalent; use whichever you prefer.

# Challenge 5 Extracting specific sections of text

`/challenge/run` prints many lines where one column is the single character of the flag. Extract that column with `cut`, then join the characters into one line with `tr -d '\n'`.

## Solution:

- Identify the column containing the flag character (most likely the 2nd column in space-separated output).
- Use `cut -d ' ' -f 2` to extract that column.
- Pipe the characters into `tr -d '\n'` to join into a single-line flag.

#### Commands run: 

```sh
hacker@data~extracting-specific-sections-of-text:~$ /challenge/run
12127 p
14143 w
12031 n
31433 .
23013 c
28954 o
1956 l
12364 l
30495 e
5296 g
15909 e
29746 {
7646 k
81 W
23737 q
9173 -
6082 4
18381 1
6386 h
18699 z
4183 G
32417 9
14282 N
25660 Q
21926 8
8099 l
4655 8
30950 d
30471 k
32191 H
17580 8
1308 b
29676 H
13567 j
22730 e
18916 H
6749 j
26126 t
17788 o
27581 .
27376 0
21655 1
4823 N
2284 x
30837 E
22388 z
13417 N
23291 x
21105 w
10768 y
10619 N
23715 w
31556 E
26444 z
25437 N
702 z
6910 E
19076 z
18623 W
13533 }
hacker@data~extracting-specific-sections-of-text:~$ /challenge/run | cut -d " " -f 2 | tr -d "\n"
pwn.college{kWq-41hzG9NQ8l8dkH8bHjeHjto.01NxEzNxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{kWq-41hzG9NQ8l8dkH8bHjeHjto.01NxEzNxwyNwEzNzEzW}
```

### Notes:

- `tr -d '\n'` joins characters; ensure you include the backslash escape inside quotes.
- Quote the space delimiter as ' ' so the shell treats it as an argument.

# Challenge 6 Sorting data

`/challenge/flags.txt` contains 100 fake flags plus the real one. When sorted alphabetically, the real flag sits at the end. Sort the file and grab that last line to reveal the flag.

## Solution:

- Sort the file so lines are in alphabetical order.
- The real flag is the final line after sorting.

#### Commands run: 

```sh
hacker@data~sorting-data:~$ sort /challenge/flags.txt
ovm.bollefd{MarMFN5y51R8oNHsIidel5BgcAB.0FL0MCOxwyNvEyMzEzV}
ovm.cnlkefe{MaqLGM5y61S8pMHrIjdel5BgcAB.0EM0MDNxwyNwDzNzEzW}
ovn.bollefe{LbrMGM4y50S8pNHsIidel4AhbAB.0FM0LDNxvyNvEzNyEzW}
.
.
.
pwn.college{MbrMGN5y61S8pNHsIjdem5BhcAB.0EM0MDOxvxNwEzNzEzV}
pwn.college{MbrMGN5y61S8pNHsIjdem5BhcAB.0FM0MDOxwyNwEzNzEzW}
```

## Flag: 

```
pwn.college{MbrMGN5y61S8pNHsIjdem5BhcAB.0FM0MDOxwyNwEzNzEzW}
```

### Notes:

- `-n` (numeric) is irrelevant here, don't use it unless sorting numbers.