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

```sh
$ id -g
1011

$ id -gn
lab6
```

> How many other groups are you a member of?

```sh
$ id -G | wc -w
3
```

### Interpreting access control metadata on files and directories

1. For the following files, determine who is the owner, which group owns the file
and characterize the group of people who can read, who can write and who can
execute the file.

> /etc/passwd

```sh
$ ls -l /etc/passwd
-rw-r--r-- 1 root root 2175 Apr 27 15:28 /etc/passwd
```

- The owner `root` (rw-) has read and write permissions
- The group `root` (r--) has read permissions 
- The other users (r--) have read permissions

> /bin/ls

```sh
$ ls -l /bin/ls
-rwxr-xr-x 1 root root 138208 Feb  7  2022 /bin/ls
```

- The owner `root` (rwx) has read, write and execute permissions
- The group `root` (r-x) has read and execute permissions
- The other users (r-x) have read and execute permissions

> ~/.bashrc

```sh
$ ls -l ~/.bashrc
-rw-r--r-- 1 lab6 lab6 3771 Apr 27 15:27 /home/lab6/.bashrc
```

- The owner `lab6` (rw-) has read and write permissions
- The group `lab6` (r--) has read permissions
- The other users (r--) have read permissions


> ~/.bash_history

```sh
$ ls -l ~/.bash_history
-rw------- 1 lab6 lab6 135 May  4 17:46 /home/lab6/.bash_history
```

- The owner `lab6` (rw-) has read and write permissions
- The group `lab6` (---) has no permissions
- The other users (---) have no permissions



2. Examine the permissions of your home directory (what option do you have to pass
to ls to examine the permissions of directories?).

We can use `ls` with the `- ld` option.

> Who is the owner and which is the owning group?

```sh
$ ls -ld ~
drwxr-xr-x 4 lab6 lab6 4096 May  4 15:32 /home/lab6
```

- Owner : lab6
- Owning group : lab6

> What is the configuration of permissions? Who can list files? Who can create files?

- The first character `d` indicates that it is a directory
- The owner `lab6` (rwx) has read (list), write (create), and execute permissions
- The group `lab6` (r-x) has read (list) and execute permissions
- The other users (r-x) have read (list) and execute permissions

3. What permissions allow you to create files in the /tmp directory?

To only create files in the `/tmp` directory we would need write permissions. Everyone has full access to it:
```sh
$ ls -ld /tmp
drwxrwxrwt 14 root root 20480 May 11 10:15 /tmp
```


### Modifying access rights

2. Conflicting permissions - Create a file (you are going to be the owner) where
the permissions are configured to not allow the owner or the group to write to the file, but allow the other users to write to the file. What does the OS do if you try to write to this file?

```sh
$ touch test.txt
$ chmod u-w,g-w test.txt
$ chmod o+w test.txt
```

```sh
$ echo "test" > test.txt
-bash: test.txt: Permission denied
```

### Giving other users access to your files

1. Is your colleague able to read the files in your home directory? If yes, why?
If no, why not?

```sh
$ ls -ld /home/lab5
drwxr-xr-x 7 lab5 lab5 4096 May  9 14:41 /home/lab5
```

From the user `lab6`, we have read and execute permissions to the home directory of `lab5`. So yes, we can read `lab5` files from `lab6` user.

2. What do you need to do so that your colleague (and maybe others) can read your
files? What do you need to do so that nobody else can read your files?

We could create a group and include in it every person that we would like to have read permissions to our home directory and exlude every other user.

```sh
$ sudo groupadd lab6_read
```

Then add every user that we want to have access to our home directory with :

```sh
sudo usermod -aG lab6_read user1
```

Finally we remove read permissions to others :
```sh
$ chmod o-r /home/lab6
```


3. In your home directory create a directory named shared . By using the commands chmod and chgrp configure the directory in such a way that only your colleague is able to read the directory and its files. You can use the groups proj_a and proj_b . Both you and your colleague are already a member of these
groups. (For the sake of this exercise, suppose that nobody else is member of this group, only you and your colleague.) Give your colleague also access rights to create new files and modify existing files. What commands did you use?

Create the directory :
```sh
$ mkdir /home/lab6/shared
```

Set the group owner of the directory created before to `proj_a` :
```sh
$ sudo chgrp proj_a ~/shared
```

Remove read, write, and execute permissions to the directory `/home/lab6/shared` :
```sh
$ chmod o-rwx /home/lab6/shared
```

Grant read and write permissions to the members of the group :
```sh
$ chmod g+rw ~/shared
```

Add `lab5` to the `proj_a` group in order to have read and write permissions to the directory :
```sh
sudo usermod -aG proj_a lab5
```


### Find

1. What does find do with hidden files or directories?

We need to use the `-a` option to "not ignore entries starting with .", so the hidden files and directories.

2. Using find display all the files in your home directory
- that end in .c , .cpp or in .sh
```sh
$ find ~ \( -name "*.c" -o -name "*.cpp" -o -name "*.sh" \)
/home/lab6/.zsh/gitstatus.sh
/home/lab6/.zsh/theme-simple.sh
/home/lab6/.zsh/theme-full.sh
/home/lab6/.zsh/title.sh
/home/lab6/.zsh/theme.sh
/home/lab6/.zsh/history.sh
/home/lab6/.zsh/generic.sh
/home/lab6/.zsh/keys.sh
/home/lab6/.zsh/alias.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/gitstatus.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/theme-simple.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/theme-full.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/title.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/theme.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/history.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/generic.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/keys.sh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/completion.sh
```


- that are executable
```sh
$ find ~ -type f -executable
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/zsh-syntax-highlighting/highlighters/main/main-highlighter.zsh
/home/lab6/.zsh/_tmp_states/arcanite_gestion/users/features/config_files/Zshconfig/.zsh/scripts/gitstatus.py
```


- that have not been modified since more than two years
```sh
$ find ~ -type f -executable -mtime +730
```


- that have not been accessed since more than two years
```sh
$ find ~ -type f -executable -atime +730
```


- that have not been accessed since more than three years and that are bigger than 3 MB (good candidates for cleanup)
```sh
$ find ~ -type f -executable -atime +1095 -size +3M
```


Display all the directories in your home directory that are called .git (probably the root of a git repository)
```sh
$ find ~ -type d -name ".git"
```

3. Suppose your current directory has no subdirectories. You want to display all files that contain the word root . Which of the two commands is correct and why:
```sh
find . -type f -exec grep -l 'root' {} \;
find * -type f -exec grep -l 'root' {} \;
```

The first command is better for our case. The first command indicates to `find` to only perform the search in the current directory and as we know that the current directory has no subdirectories there is no need to use `*` to accomplish the same result but in a recursively way.

## Script task 2-4:  
```
#!/bin/bash  
#  
# Description : Display world-writable files for a specific directory. A world-writable file is a file with the write bit set for others. 
# It additionally suggest to the user to fix this risk by removing this access for others.  
#  
# Authors : Pablo Urizar, Vincent Peer  
#  
# Date : 09.05.2023  
```

if [[ $# -ne 1 ]]  
then  
  echo "Error: missing argument. Please specify a directory" >&2  
  exit  1  
  
elif [[ ! -d "$1" ]]  
then  
  echo "Invalid directory" >&2  
  exit 1  
  
else  
  echo "The following files/directories are world-writable:"  
  find "$1" -perm -o+w  
fi   
  
read -p "Do you want the permissions to be fixed (y/n)?" response  
  
if [[ "${response}" == "y" || "${response}" == "yes" ]]  
then  
  find "$1" -perm -o+w -exec chmod o-w {} \;  
  echo "Offending permissions have been removed"  
else  
  echo "Offending permissions unchanged"  
fi  

echo "The following files/directories are writable for groups:"  
find "$1" ! -group $USER -perm -g+w  
