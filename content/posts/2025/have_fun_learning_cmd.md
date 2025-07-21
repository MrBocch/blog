+++
title = "Have fun learning the command line"
date = "2025-07-21T12:08:20-06:00"
author = ""
authorTwitter = "" #do not include @
cover = "img/sailor.jpg"
# tags = ["", ""]
keywords = ["", ""]
description = "Learn the command line while having fun!"
showFullContent = false
readingTime = false
hideComments = false
color = "" #color from the theme settings
+++

In a previous article I mentioned that everyone should
rent a VPS for selfhosting an ecetera of things, but
if you dont know the command line what can you do?

## Overthewire.org
The bandit wargame on {{< rawhtml >}}
    <a href ="https://overthewire.org/" target = "_blank">this website</a>
{{< /rawhtml >}}, is a fun an interactive way to learn basic shell commands in the form
of a interactive puzzels.

The basic premise of this, each level is a user in a linux server. In order to move
onto the next level you must find the password for the user. You find the password
by

{{< rawhtml >}}
<!-- dont look at this, im storing pwds for my sake DONT LOOK
     the passwords would probably be outdated by the time you read this.
-->
{{< /rawhtml >}}

## 🚨🚨🚨 SPOILERS 🚨🚨🚨

Im going to write down how I solved each level. If you haven't atleast tried
it go try it before reading this.

### lvl0 -> lvl1

The first level is to ssh into server on specified port and user

```bash
ssh <user>@<IP or DOMAIN NAME> -p <PORT>
```

You are prompted for the password, the password is the same as username.

The first level is easy enough, its in the 'readme' file in user's home directory.
I use `cat` to output file because its the easiest to copy and paste password for the next level.

{{< rawhtml >}}
<!--
    ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If
 -->
{{< /rawhtml >}}

### lvl1 -> lvl2

Now you repeat the same proccess from the first level but for user bandit1.
I figured it be quicker to just change user with `su <user>` but for whatever reason it does not work.

On this level you must open a file named '-', this causes issues because how does a cli
know how to differentiate between a file named '-' or if its a command line flag?

I know of one solution, you could refer to the file by its full path name.

```bash
cat ./-
```

This reminded me of this story in *The Unix Haters Manual*, there is this case
were if you named a file '-rf' it would take it as a argument and delete everthing in the directory.

{{< rawhtml >}}
<!--
    263JGJPfgU6LtdEvgfWU1XP5yac29mFx
-->
{{< /rawhtml >}}

### lvl2 -> lvl3

This challenge is to open a file with spaces in its name.

You can address a file with spaces in its name by escaping the space character with \
or enclosing the name with quotes

```bash
cat name\ of\ file
#or
cat "name of file"
```

{{< rawhtml >}}
<!--
   MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx
-->
{{< /rawhtml >}}

### lvl3 -> lvl4

The challenge is listing hidden files.

In UNIX file system, every directory has to default directories the
- '.' current directory
- '..' its parent directory

To make `ls` more terse (i think it was brian kernighan) the developer decided to
ignore file the first character was a '.' so that these default directories were ignored.

```python
# psuedocode example
for name in current_dir:
    if name[0] == '.':
        continue
    else:
        print(name)
```

The -a flag is used to show all files

```bash
ls -a inhere/
cat inhere/<name of file>
```

{{< rawhtml >}}
<!--
    2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ
-->
{{< /rawhtml >}}

### lvl4 -> lvl5

This is sort of a recap of the lvl0 -> lvl1 but
they are multiple files (file00 .. file09).

Im sure this is the most naive way of doing it but they are only 10 files
so its not a big deal.

```bash
cat ./-file00
# incase its not the file use up arrow to quickly rewrite command
```

The files seem to have special characters that mess with the terminal, the reset command
resets everything back to what they were like.

{{< rawhtml >}}
<!--
    4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw
-->
{{< /rawhtml >}}

### lvl5 -> lvl6

This time were are tasked to find a file that is

- human-readable
- 1033 bytes in size
- not executable

This seems like a trivial because
`ls -lh` would list out file permissions '-rwxr---' and file sizes but
they are 20 folders each with 6 files. Doing this by hand wont do anymore.

Actually I forgot that there could be hidden files so they are actually 9 files per folder.

So how do we go about finding the password?
