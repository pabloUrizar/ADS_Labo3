# ADS Lab 07 - Account Management

Authors: Vincent Peer, Pablo Urizar

Date: May the 25th, 2023

The **lab5** account on the server contains the elements asked for this lab.

## Task 0: Examine the setup of your own account

> Examine your account by using the command id and by looking into the files /etc/passwd and /etc/group . What is its principal group? What other groups is the account a member of? What is the UID of the account and the GID of the principal group?

```sh
$ id  
uid=1000(vpeer) gid=1000(vpeer) groups=1000(vpeer),4(adm),20(dialout),24(cdrom),25(floppy),27(sudo),29(audio),30(dip),44(video),46(plugdev),116(netdev),1001(proj_a),1002(proj_b)  
```
- The principal group is vpeer
- The account is also member of the following groups : adm, dialout, cdrom, floppy, [...], proj_a, proj_b
- The UID is 1000 and de GID of the principal group is also 1000

> Which skeleton files have been copied?

Files in /etc/skel, so .bash_logout, .bashrc and .profile.

## Task 1: Create user accounts

1. Create the groups jedi and rebels . Before creating them verify that they do not yet exist.

```sh
$ grep -e jedi -e rebels /etc/group # Look for existing groups
$ sudo addgroup jedi
$ sudo addgroup rebels
```

2. Create the following user accounts with default home directories and login shell (for example account luke should have home directory /home/luke and a bash shell).

```sh

```

> What option do you need to specify to have useradd create a home directory? 

```sh

```

> What is the default login shell for users created with useradd ? What command should we use to change the default login shell from /bin/sh to /bin/bash ?

```sh

```

Before creating them verify that they do not yet exist.

> Account luke , assigned to groups jedi (principal) and rebels .

```sh

```

> Account vader , assigned to group jedi (principal).

```sh

```

> Account solo , assigned to group rebels (principal).

```sh

```

3. Set a password for the account luke .

```sh

```

4. Test the account luke . Verify that the user can log in and create files. Verify that the user cannot access sensitive system information such as the file /etc/shadow .

```sh

```

5. Use su to change your account to that of vader . Test if the user vader has access to the files in the home directory of user luke .

```sh

```

## Task 2: Change group membership

>  Create the account leia without assigning it a principal group. After it was created, which principal group did it get assigned?

```sh

```

> Make leia member of the group rebels (as secondary group).

```sh

```

> Make leia leave the group rebels and join the group jedi instead.

```sh

```

> Make leia leave any secondary group.

```sh

```

## Task 3: Give a user sudo rights

Questions:

a) Which line in /etc/sudoers gives the members of the group sudo the right to execute any command?

b) How would you have to modify this line so that users can use sudo without typing a password (this is in general not recommended, but can be handy sometimes).

Perform the following steps and give in the lab report the commands you used.

> Give the account luke sudo rights.

```sh

```

> Test the new rights. Verify that luke can read the file /etc/shadow using sudo.

```sh

```

> Remove sudo rights from the account luke.

```sh

```

## Task 4: Remove a user account

1. Remove the account leia , but do not delete the home directory yet.

2. Inspect the home directory (look at the file metadata). What has changed?

3. Suppose the user leia has created other files on the system, but you do not know where they are. How would you systematically scan the whole system to find them?

4. Remove the home directory manually.
