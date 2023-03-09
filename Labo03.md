# ADS Lab 03 - Pipeline

Authors: Vincent Peer, Pablo Urizar

Date: March 9, 2023

## Task 1: Exercises on redirection
1. Run the following commands and tell where stdout and stderr are redirected to.  
a. ./out > file  
> stdout is redirected to the file. stderr still points to the command prompt.  
b. ./out 2> file   
> stdout still points to the command prompt. stderr is redirected to the file.   
c. ./out > file 2>&1   
> stdout is redirected to the file. stderr is redirected to stdout, thus to the file, so both to the file.  
d. ./out 2>&1 > file  
> First, stderr is redirected to stdout, thus to the command prompt. Then stdout is redirected to the file but stderr is still pointing to the command prompt.  
e. ./out &> file  
> stdout and stderr are both redirected to the file.   

2. What do the following commands do?

3. Write commands to perform the following tasks:

## Task 2: Log analysis


## Task 3: Conversion to CSV
