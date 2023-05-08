# ADS Lab 06 - Le contrôle d'accès

Authors: Vincent Peer, Pablo Urizar

Date: May the 4th, 2023

## Task 1: Exercises

### Interpreting account and group information 
> What is your UID and what is your account name?
```sh
$ id

uid=1007(lab6) gid=1011(lab6) groups=1011(lab6),1012(proj_a),1013(proj_b)
```

> What is the GID of your primary group ("groupe principal") and what is its
name? 


> How many other groups are you a member of?


### Interpreting access control metadata on files and directories

1. For the following files, determine who is the owner, which group owns the file
and characterize the group of people who can read, who can write and who can
execute the file.

> /etc/passwd

> /bin/ls

> ~/.bashrc

> ~/.bash_history


2. Examine the permissions of your home directory (what option do you have to pass
to ls to examine the permissions of directories?).

> Who is the owner and which is the owning group?

> What is the configuration of permissions? Who can list files? Who can create files?


3. What permissions allow you to create files in the /tmp directory?


### Modifying access rights

2. Conflicting permissions - Create a file (you are going to be the owner) where
the permissions are configured to not allow the owner or the group to write to the file, but allow the other users to write to the file. What does the OS do if you try to write to this file?

### Giving other users access to your files

1. Is your colleague able to read the files in your home directory? If yes, why?
If no, why not?

2. What do you need to do so that your colleague (and maybe others) can read your
files? What do you need to do so that nobody else can read your files?

3. In your home directory create a directory named shared . By using the commands chmod and chgrp configure the directory in such a way that only your colleague is able to read the directory and its files. You can use the groups proj_a and proj_b . Both you and your colleague are already a member of these
groups. (For the sake of this exercise, suppose that nobody else is member of this group, only you and your colleague.) Give your colleague also access rights to create new files and modify existing files. What commands did you use?


### Find

1. What does find do with hidden files or directories?

2. Using find display all the files in your home directory
- that end in .c , .cpp or in .sh
- that are executable
- that have not been modified since more than two years
- that have not been accessed since more than two years
- that have not been accessed since more than three years and that are bigger than 3 MB (good candidates for cleanup)

Display all the directories in your home directory that are called .git (probably the root of a git repository)

3. Suppose your current directory has no subdirectories. You want to display all files that contain the word root . Which of the two commands is correct and why:
```sh
find . -type f -exec grep -l 'root' {} \;
find * -type f -exec grep -l 'root' {} \;
```



## Script de Vincent en progression:
#!/bin/bash

if [[ $# -ne 1 ]]  
then  
        echo "Error: missing argument"  
        echo "Please specify a directory"  
        echo  1  
elif [[  ]]  


echo "The following files/directories are world-writable:"  


find test_dir/ -perm -a+w  
