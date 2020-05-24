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
`-name pattern`: find files and directories that match pattern
`-iname pattern`: find files and ignore case
`-ls`: performs an `ls` on each of the found items
`-mtime n-days`: find files that n-days old
`-size num`: find files of size num
`-newer some_file`: find files that are newer than some_file
`-exec command {} \;`: run command against all files that are found

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

### sort command

Sort the content and show

```
cat my-file.txt
sort my-file.txt # will not actually change the content sort by first column
sort -u my-file # sort and remove duplicate line
sort -ru my-file # reverse sort and remove dupes
sort -u -k2 my-file # sort by second column

```

### tar command

Create a collection of files

```
tar [-] c|x|t f tarfile [pattern]  # "-" is optional. You don't need to add "-" for options but you can
```

Create, extract of list contents of a tar archive using pattern, if supplied

- c: create a tar archieve
- x: extract files from archieve
- t: display table of contents(list)
- v: verbose
- z: Use compression
- f file: Use this file

```bash
tar cf mytar.tar mydir  # create a tar file called mytar.tar for mydir
tar tf mytar.tar  # display the list of directories for .tar file
tar xvf mytar.tar # extract the content of mytar.tar and put them in current directory, with v for verbose
```

To save space, gzip the file

- gzip: compress files
- gunzip: Uncompress files
- gzcat: contcatenates compressed files
- zcat: cintatenates compressed files

```
gzip filename  # will give you filename.gz
du -h filename.gz  # check the size
gunzip filename.gz  # unzip it

```

Combine tar and gzip

```
tar zcf mytar.tgz  mydir  # tar and zip mydir and create a file called mytar.tgz - tgz is a convention, also you might see ".tar.gz" for same thing

```

## Wildcards

- "\*": match everything
- "?": match one charecter
- "[]": A character clas
  - "[aeiou]": match any one of "aeiou". Match only one
  - "ca[nt]\*: match "can", "cat", "candy", "catch"
- "[!]": match anything that is not in the bracket. Match one character.
  - "[!aeiou]\*": will match baseball, criket. Will not match apple, egg
- A list of named Charater classes:
  - "[[:alpha:]]" : match all aphpabetic
  - "[[:alnum:]]" : match alphanumeric
  - "[[:digit:]]": match all digits
  - "[[:lower:]]: match all lowercase
  - "[[space]]": match all spacesm and link breaks

```bash
ls ????.py  # abcd.py, efgh.py will match

ls [aeiou]* # list file starting with a, e, i, o, or u
mv *.py /mydir  # move everything .py to mydir
```

## Redirection

Standard input(stdin) 0 (number here is called file descriptor)
Standard output(stdout) 1
Standard error(stderr) 2

- `>`: redirects stdout to a file, overwrites existing content
- `>>` redirects stdout to a file, append to existing content
- `<`: redirects input from a file to a command

You can use `$` to signal that you are using file descriptor.

- `>/dev/null`: redirect output to nowhere. This is called **Null device**

Examples:

```
ls -l > file.txt  # redirect the output to file.txt
ls -l >> file.txt # append the output to file.txt
sort < file.txt  # redirect the file content into the command

# use file descriptor
ls -l 1> file.txt  # this is really the same as "ls -l > file.txt"
sort < files.txt > sorted_files.txt # redicrt the output to sorted_files.txt

ls file.txt file_not_exist.txt > out.txt # only file.txt will be redirected to out.txt
ls file.txt file_not_exist.txt 1>out.txt 2>err.txt # redirect the stderr to err.txt
ls file.txt file_not_exist.txt > out.both 2>&1 # redirect stderr to stdout (basically all to stdout)
ls file.txt file_not_exist.txt 2>/dev/null # ignore stderr by sending it to nowhere

```
