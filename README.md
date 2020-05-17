# Linux

## Installation

Install Virtualbox and Centos. Centos is one of the Linux distros which belongs to redhat (RHEL).

For Virtualbox, allocate proper memory. I would do 2 GB. For hard disk, use existing virtual hard disk file.

Start your virtualbox using the Centos image. Password is "adminuse" - this is set by the image provider

## Some commands

`cd -`: take you to the previous directory
`mkdir [-p] directory`: [-p] means the option p is optional. `-p` means parent. If you need to create a parent directories you need to use `-p`.

```bash
mkdir -p dir1/dir2/dir3
```

### Useful ls command

`ls -F`: will tell you what file types. E.g., / is directoru. @ means it's a link(Symbolic Link or Symlink for short), and \* means it's an executable
`ls -t`: list files by time
`ls -r`: reverse order
`ls -latr`: long list, all files, by time and reverse (latest at the end of the list)

## Permissions

For example you see a permission like this:

```
-rw-rw-r--
```

- The symbol "-" means it's a regular file
- "d" means it's a directory
- "l" means it's a simbolic link
- "r" means read
- "w" means write
- "x" means execute

Permission categories

- "u" means user (it's the owner of the file)
- "g" means group
- "o" means other
- "a" means all

So `-rw-rw-r--` means it's a regular file, User can read and write, Group can read and write, Other can only read. This always follow the same order: User, Group, Other

### chmod

Change Permission

```
chmod g+w myfile # give group write access for myfile
chmod u+x myfile # make executable for user. For python file you will add something like to specify the python #!/usr/bin/python
```

Numberic based permission

- 0 - value off
- 4 - read
- 2 - write
- 1 - execute

7 = 4 + 2 + 1 (all rights). There are 8 possible ways so it's called _Octal_
So for example:

```
chmod 700 myfile  # user can read, write, and execute. Group and Other have none permission for myfile

chmod 755 myfile # user can do anything. Group and Other can read and execute.


```

### chgrp

Change Group.

```bash
groups # show all the groups you are in

```

You can change the group for the file

```
chgrp someGroup myfile # change myfile's group to someGroup

```

### File Creation Mask

This means the default permission when files are creaetd. Normally set by the admin.

If no mask were used permissions would be:

- 777 for directories
- 666 for files

You can change the mask by using `umask [-S] [mode]` command. "-S" will display permission in Symbols (charecters like rwx) rather than numbers.
`umask` is displaying as "substraction from the default". For example,

```
mkdir testumask
cd testumask
umask   # show your default -> 022 means 755
umask -S # u=rwx,g=rx,o=rx
umask 007 # change mask to 007 -> means 770
umask # 007

```

### find command
```
find directory -option args
```

Options:
` -name pattern`: find files and directories that match pattern
` -iname pattern`: find files and ignore case
` -ls`: performs an `ls` on each of the found items
` -mtime n-days`: find files that n-days old
` -size num`: find files of size num
` -newer some_file`: find files that are newer than some_file
` -exec command {} \;`: run command against all files that are found

For example,
```bash
find /bin -iname "*v"
find . -mtime -1  # files that were 1 day old
find . -mtime +10 -mtime -13  # more than 10 days old but less then 13 days old
find . -iname "*.py" -ls # show ls for everything that ends with .py
find . -size +1M # files greater than 1MB 
find . -type d -newer file.txt # find directory that's newer than file.txt
find . -name "*py" -exec file {} \;  # find every .py file and execute "file" command
```

#### locate command
**This does not work for Mac**
Also there is `locate` command which does the same thing as "find" but faster since it uses index.
```
locate .py
```

### View file

#### cat, more, less, head, tail
```
head -10 file.txt
tail -10 file.txt
```

To follow files in real time:
```
tail -f file # -f for follow, this will be useful for following logs

```
