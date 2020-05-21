---
title: Follow the Tail
subTitle: Mastering the tail command can be super handy when it comes to following the progress of a process via its output logs.
cover: ./uriel-soberanes-HwMxe5F6KtA-unsplash.jpg

tags: ["tail", "linux", "command line"]
---

My command line skills are limited to the terms I learned while working my way through the excellent [Command-Line Murder Mystery](https://github.com/veltman/clmystery) challenge.

![Mystery Challenge](./joao-silas-I_LgQ8JZFGE-unsplash.jpg)

Commands like `head` and `tail` were used there to expose evidence files, in order to get a sense of what they contained. I've moved onto `less` (and it's lesser cousin `more`) for paging through a file's content but have unfortunately never since had to scour through anything as interesting as evidence files.

In my working life I am happiest when lodged firmly in GUI land. I like VSCode and even though it has an integrated terminal, I'm content to do most of my work away from the shell.

Recently though, my work has migrated to a remote server which has not been sullied by graphical interfaces and I am having to come to terms with unix commands and vim (here's a cool [vim cheatsheet](http://web.mit.edu/merolish/Public/vi-ref.pdf) where I discovered how to yank.)

I'm running a process which takes yonks and keeps failing about 3/4's of the way through. Who knows why - it's a mystery. Or at least it may as well be as my logs are a bit useless.

I'm basically running bulk inserts into an elasticsearch index, running 500 documents at a time. In my wisdom I thought a handy log message would be "Hey Whatapalaver - you've just bulk imported 500 documents - you coding rockstar!"

Frankly that's pretty useful and uplifting the first time round but when it crashes out somewhere between 1500 and 1763 repetitions into the run, it shows its weakness pretty quickly.

Needless to say I've since improved my log messages with a little counter that tells me the running tally of indexed documents and when the process finishes or crashes it dumps the guilty document.

Initially when I ran my process, all my many print statements were displayed on the terminal for me to view live but as my process took so long to complete I found my ssh session was being timed out and crashing my process uneccessarily. I therefore prefixed my command with the unix command `nohup`. I'm reading this as "no hangup" and it just means that your process will continue to run even after you log out (or get forcibly ejected).

`nohup` also directs all the output messages to a log file 'nohup.out' which is where `tail` comes in.

## Tail Syntax and Examples

### tail last 10 lines

In order to check on progress I was using `tail nohup.out` to display the (default) last 10 lines of my log file.

### tail -n lines

But maybe you want 20 lines - just use the -n flag

`tail -n 20 nohup.out`

### tail multiple log files

Tail all log files at once:

`tail *.log`

or specific files:

`tail first.log second.log`

```
==> first.log <==
Hello World!

==> second.log <==
Goodbye World!
```

### tail file with live updates (follow)

Following a file with tail is just what I needed. The process takes ages and I didn't want to keep repeating my tail command. Adding the -f (follow) flag means that stdout is refreshed as the log file changes:

`tail -f nohup.out`

### tail and grep combined for filtering crappy log messages

While I added more useful log messages (ie a counter), I forgot to remove my "coding rockstar" message and was getting an output like this:

```
Hey Whatapalaver - you've just bulk imported 500 documents - you coding rockstar!
Documents indexed so far 1223500

Hey Whatapalaver - you've just bulk imported 500 documents - you coding rockstar!
Documents indexed so far 1224000

Hey Whatapalaver - you've just bulk imported 500 documents - you coding rockstar!
Documents indexed so far 1224500

Hey Whatapalaver - you've just bulk imported 500 documents - you coding rockstar!
Documents indexed so far 1225000

Hey Whatapalaver - you've just bulk imported 500 documents - you coding rockstar!
Documents indexed so far 1225500

```

In order to display just the professional and useful log, I introduced the wonders of grep:

`tail -f nohup.out | grep indexed`

to give:

```
Documents indexed so far 1223500
Documents indexed so far 1224000
Documents indexed so far 1224500
Documents indexed so far 1225000
Documents indexed so far 1225500
```
