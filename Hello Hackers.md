
# Challenge 1 Intro To Commands

In this challenge, you will invoke your first command! When you type a command and hit enter, the command will be invoked, as so:

```sh
hacker@dojo:~$ whoami  
hacker
hacker@dojo:~$
```
Here, the user executed the whoami command, which simply prints the username (hacker) to the terminal. When the command terminates, the shell once again displays the prompt, ready for the next command.

## Solution:

To complete this challenge, the user needs to run the hello command in the remote dojo terminal. This prints the flag.

#### Commands run: 

```sh
$ hello
```

## Flag: 

```
pwn.college{s7EfivgBIIFROJc351RPiN8p274.QX3YjM1wyNwEzNzEzW}
```

### Notes:

-The `$` at the end of the prompt indicates that the user is not an administrative user.
-Linux commands are case sensitive: `hello` is different from `HELLO`.
-First remote command challenge completed successfully.

# Challenge 2 Intro To Arguments

Let's try something more complicated: a command with arguments, which is what we call additional data passed to the command. When you type a line of text and hit enter, the shell actually parses your input into a command and its arguments. The first word is the command, and the subsequent words are arguments. Observe:

```sh
hacker@dojo:~$ echo Hello
Hello
hacker@dojo:~$
```

In this case, the command was echo, and the argument was Hello. echo is a simple command that "echoes" all of its arguments back out onto the terminal, like you see in the session above.

Let's look at echo with multiple arguments:

```sh
hacker@dojo:~$ echo Hello Hackers!
Hello Hackers!
hacker@dojo:~$
```

In this case, the command was echo, and Hello and Hackers! were the two arguments to echo. Simple!

## Solution:

After the terminal comes up, the user needs to invoke `hello` command with argument `hackers`

#### Commands run: 

```sh
$ hello hackers
```

## Flag: 

```
pwn.college{QqqDwyRIvOtbgh2ozurysqluJ7d.QX4YjM1wyNwEzNzEzW}
```

### Notes:

-When you type a line of text and hit enter, the shell actually parses your input into a command and its arguments. The first word is the command, and the subsequent words are arguments.
-`echo` is a simple command that "echoes" all of its arguments back out onto the terminal.
-Learned how commands take arguments and how they affect output.
-First challenge with a command argument completed successfully.

# Challenge 3 Command History

You're going to type a lot of commands, and typing everything from scratch can be annoying. Luckily, the shell saves a history of every command you invoke.

You can scroll through those saved commands with the up/down arrow keys. 


## Solution:

Here the flag is already in our command history (immediate previous command)

#### Commands run:

Press the `up` arrow to scroll to the last command


## Flag: 

```
pwn.college{MX9cprszJK6bpkOXYkNwyPMdAx3.0lNzEzNxwyNwEzNzEzW}
```

### Notes:

-The shell keeps a history of all commands you’ve run.
-You can scroll through previous commands using the `up`/`down` arrows.
-This saves time and prevents mistakes when repeating commands.