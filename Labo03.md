# ADS Lab 03 - Pipeline

Authors: Vincent Peer, Pablo Urizar

Date: March 9, 2023

## Task 1: Exercises on redirection

### 1. Run the following commands and tell where stdout and stderr are redirected to.  
> a. ./out > file  

stdout is redirected to the file. stderr still points to the command prompt.  
> b. ./out 2> file   

stdout still points to the command prompt. stderr is redirected to the file.   
> c. ./out > file 2>&1   
 
stdout is redirected to the file. stderr is redirected to stdout, thus to the file, so both to the file.  
> d. ./out 2>&1 > file  
 
First, stderr is redirected to stdout, thus to the command prompt. Then stdout is redirected to the file but stderr is still pointing to the command prompt.  
> e. ./out &> file  
 
stdout and stderr are both redirected to the file.   


### 2. What do the following commands do?
> a. cat /usr/share/doc/nano/README | grep -i edit

It will display the lines that contain the word "edit" in the provided README file.

> b. ./out 2>&1 | grep –i eeeee

stderr will be redirected to stdout so both will be displayed in the command prompt. From the obtained result, we will keep only the lines that contain "eeeee".

> c. ./out 2>&1 >/dev/null | grep –i eeeee

stderr will be redirected to stdout but it does not change anything because stderr was already directed to the command prompt output. Then, only the lines that contain "eeeee" will be displayed.

### 3. Write commands to perform the following tasks:
> a. Produce a recursive listing, using ls , of files and directories in your
home directory, including hidden files, in the file /tmp/homefileslist .

ls -Rla ~ > /tmp/homefileslist

> b. Produce a (non-recursive) listing of all files in your home directory whose names end in .txt , .md or .pdf , in the file /tmp/homedocumentslist . The command must not display an error message if there are no corresponding files.



## Task 2: Log analysis


## Task 3: Conversion to CSV
> Extract all dates from the log file

```bash
grep -oE '\[[0-9]{2}/[A-Za-z]{3}/[0-9]{4}' ads_website.log | cut -c 2-12 > dates.txt
```

> Sort dates first by year, then by me, and finally by day

```bash
sort -t'/' -k3 -k2M -k1 dates.txt > dates_tries.txt
```

> Count the occurrences of each date and save them to an access.csv file

```bash
cat dates_tries.txt | uniq -c | awk '{print $2","$1}' > access.csv
```
